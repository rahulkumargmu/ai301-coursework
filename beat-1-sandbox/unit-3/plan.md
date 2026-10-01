# Plan: issue #72, verify_password should return False for a non-bcrypt hash

Issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72

## Diagnosis

`verify_password()` in `core/security.py` passes the stored hash straight to `pwd_context.verify()` and lets the exception escape. The posted reproduction on commit `2f4e82f` shows three facts:

- `"not_a_valid_bcrypt_hash"` and `""` raise `passlib.exc.UnknownHashError: hash could not be identified` from `core/security.py:37`.
- `"$2b$12$abc"` raises `ValueError: salt too small (bcrypt requires exactly 22 chars)`, not `UnknownHashError`.
- `UnknownHashError` is a `ValueError` (`issubclass` printed `True`).

The controls on a real bcrypt hash returned `True` for the right password and `False` for a wrong one, so hashing valid passwords is fine. The missing handler is the cause. Catching only `UnknownHashError` would still let the truncated `$2b$` hash raise.

Quoted from the posted repro, step 3:

```
control, right password -> True
control, wrong password -> False
'not_a_valid_bcrypt_hash'   -> passlib.exc.UnknownHashError: hash could not be identified
''                          -> passlib.exc.UnknownHashError: hash could not be identified
'$2b$12$abc'                -> builtins.ValueError: salt too small (bcrypt requires exactly 22 chars)
UnknownHashError is a ValueError: True
```

## Scope

In scope: make `verify_password()` return `False` when `pwd_context.verify()` raises `ValueError`, and remove the strict `xfail` on `test_verify_with_wrong_hash_format` so that test asserts `False` for real. Add the empty string and the truncated `$2b$` hash to that test, because the repro showed those are the same bug.

Out of scope: the passlib `bcrypt.__about__` stderr traceback, the `vector-db` container crash, `hash_password()`, JWT helpers, and any change to how a valid bcrypt hash is verified.

## Files

- `core/security.py`: the `verify_password` body at the `pwd_context.verify` call.
- `tests/unit/test_security.py`: `TestSecurity.test_verify_with_wrong_hash_format`.

## Approach

1. In `verify_password`, call `pwd_context.verify` inside `try`. On `ValueError`, return `False`. Leave the `True`/`False` result of a successful verify unchanged.
2. Delete the `@pytest.mark.xfail(strict=True, ...)` marker on `test_verify_with_wrong_hash_format`.
3. Extend that test to call `verify_password("password", stored)` for `"not_a_valid_bcrypt_hash"`, `""`, and `"$2b$12$abc"`, and assert each result is `False`. Keep a valid-hash control in the existing hash tests; do not change those.

## Test plan

Re-run the unit 2 repro on this tree.

Before, already posted: `pytest tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format -v --runxfail --tb=short` fails with `passlib.exc.UnknownHashError` from `core/security.py:37`. The direct-call script prints the three exceptions above, and the two controls print `True` and `False`.

After:

- `pytest tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format -v --tb=short` passes, with no `xfail` and no exception. The assertion is `result is False`.
- The same direct-call script prints `False` for all three malformed stored values, and still prints `True` / `False` for the two controls.
- `pytest tests/unit/test_security.py -q` stays green, so valid-hash verification did not change.

## Risks and unknowns

Catching `ValueError` is wider than `UnknownHashError`. The repro is why: the truncated hash raises a plain `ValueError`, and `UnknownHashError` is a subclass, so the narrower handler misses a case the test input already hits. I have not seen a valid hash raise `ValueError`; the controls returned booleans. If a later passlib change raises `ValueError` for a reason other than an unusable hash, this function would return `False` for that too. I am not changing passlib.

## Deviations

Nothing changed. The code I built is the change this plan describes: `verify_password()` catches `ValueError` from `pwd_context.verify()` and returns `False`, the strict `xfail` is gone, and the test asserts `False` for the three stored values from the reproduction. I did not widen or narrow the change while writing it. The plan held.
