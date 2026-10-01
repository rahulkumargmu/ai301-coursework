# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

rahulkumargmu

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5923171625

Text as posted:

````markdown
Plan for #72, from my own reproduction on `2f4e82f` (comment above). `verify_password()` lets `passlib.exc.UnknownHashError` escape for `"not_a_valid_bcrypt_hash"` and `""`, and a plain `ValueError` ("salt too small") escape for `"$2b$12$abc"`. A real bcrypt hash still returns `True` or `False`. `UnknownHashError` is a `ValueError`, so catching only `UnknownHashError` would miss the truncated hash.

Plan: in `core/security.py`, `verify_password()` catches `ValueError` from `pwd_context.verify()` and returns `False`. I will drop the strict `xfail` on `test_verify_with_wrong_hash_format` and assert `False` for those three stored values. I will not change `hash_password` or valid-hash verification. The passlib `bcrypt.__about__` warning and the `vector-db` container crash stay out of this change.

After the change I expect the same pytest command to pass without `--runxfail`, and the direct-call script to print `False` for the three bad hashes while the two controls stay `True` and `False`.

I used Claude to help draft this plan. The reproduction output it relies on is from my machine.
````

---

## Your branch

**Branch**

fix/72-fail-closed

**Evidence**

Before, on `main` at `2f4e82f`, the same tree as the unit 2 report. The strict xfail still hides the failure:

```bash
pytest tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format -v --tb=line
```

```
tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format XFAIL [100%]
======================== 1 xfailed, 2 warnings in 0.50s ========================
```

The same test with the marker ignored:

```bash
pytest tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format -v --runxfail --tb=short
```

```
tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format FAILED [100%]

=================================== FAILURES ===================================
_______________ TestSecurity.test_verify_with_wrong_hash_format ________________
tests/unit/test_security.py:227: in test_verify_with_wrong_hash_format
    result = verify_password("password", wrong_hash)
             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
core/security.py:37: in verify_password
    return bool(pwd_context.verify(plain_password, hashed_password))
                ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.venv/lib/python3.11/site-packages/passlib/context.py:2343: in verify
    record = self._get_or_identify_record(hash, scheme, category)
             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.venv/lib/python3.11/site-packages/passlib/context.py:2031: in _get_or_identify_record
    return self._identify_record(hash, category)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.venv/lib/python3.11/site-packages/passlib/context.py:1132: in identify_record
    raise exc.UnknownHashError("hash could not be identified")
E   passlib.exc.UnknownHashError: hash could not be identified
FAILED tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format
======================== 1 failed, 2 warnings in 0.28s =========================
```

Direct calls, same venv:

```bash
python - <<'EOF'
from passlib.exc import UnknownHashError
from core.security import hash_password, verify_password

good = hash_password("password")
print("control, right password ->", verify_password("password", good))
print("control, wrong password ->", verify_password("nope", good))

for stored in ["not_a_valid_bcrypt_hash", "", "$2b$12$abc"]:
    try:
        print(f"{stored!r:27} ->", verify_password("password", stored))
    except Exception as e:
        print(f"{stored!r:27} -> {type(e).__module__}.{type(e).__name__}: {e}")

print("UnknownHashError is a ValueError:", issubclass(UnknownHashError, ValueError))
EOF
```

```
control, right password -> True
control, wrong password -> False
'not_a_valid_bcrypt_hash'   -> passlib.exc.UnknownHashError: hash could not be identified
''                          -> passlib.exc.UnknownHashError: hash could not be identified
'$2b$12$abc'                -> builtins.ValueError: salt too small (bcrypt requires exactly 22 chars)
UnknownHashError is a ValueError: True
```

After, on `fix/72-fail-closed`. The xfail is gone, so the test runs for real:

```bash
pytest tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format -v --tb=short
```

```
tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format PASSED [100%]
======================== 1 passed, 2 warnings in 0.38s =========================
```

The same direct-call script:

```
control, right password -> True
control, wrong password -> False
'not_a_valid_bcrypt_hash'   -> False
''                          -> False
'$2b$12$abc'                -> False
UnknownHashError is a ValueError: True
```

The rest of the security unit file:

```bash
pytest tests/unit/test_security.py -q --tb=line
```

```
.........................                                                [100%]
25 passed, 2 warnings in 7.34s
```

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Partial run, before the cause check was revised: `python3 run_eval.py --rubric ~/.claude/skills/plan-check/rubric.md --evidence ~/.claude/skills/plan-check/references/evidence-guide.md --include-calibration --only pkg-01,pkg-02,pkg-04,pkg-05,pkg-09,pkg-10,pkg-14,pkg-15,pkg-18,pkg-20,calib-03`. Result: `agreement: 9/10 scored items`. `pkg-14` (gold `accept`) was graded `reject` on `cause-grounded`. It was a partial run, so it printed no bar verdict.
2. Canary run after revising `cause-grounded`, with no further edits after it: `python3 run_eval.py --rubric ~/.claude/skills/plan-check/rubric.md --evidence ~/.claude/skills/plan-check/references/evidence-guide.md --include-calibration --only pkg-01,pkg-02,pkg-04,pkg-07,pkg-11,pkg-14,pkg-16,pkg-20,calib-03`. Result: `agreement: 8/8 scored items` and `categories: clear-accept 2/2  thread-convention 2/2  wrong-cause 4/4`. `pkg-14` agreed. `calib-03` matched (`reject` / `reject`). Partial, so no bar verdict.
3. Confirming full run, with no rubric, evidence-guide, or procedure changes after run 2: `python3 run_eval.py --rubric ~/.claude/skills/plan-check/rubric.md --evidence ~/.claude/skills/plan-check/references/evidence-guide.md --save-run eval-run.txt`. Result: `agreement: 20/20 scored items  (bar: 18/20: PASS)` and `categories: clear-accept 7/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`. This run wrote the committed `eval-run.txt`, and its agreement line matches.

**Package analysis**

`pkg-20` (ghostty-org/ghostty#11261, category `thread-convention`). The gold label is `reject`: "excellent bounded plan that follows the thread's direction, but the comment contains no AI-use disclosure and ghostty's stated policy requires disclosing all AI usage; every package here is treated as AI-assisted work". My rubric's verdict was also `reject` (`pkg-20  reject  reject   yes`).

The plan itself passed the other required checks. The full run's `--out` results show `cause-grounded: pass` ("Control removing hyperlink start (no mid-print growth) passes, confirming growth-during-print as the stale-prev trigger the plan names"), `scope-bounded: pass`, `executable: pass`, and `test-decisive: pass`. `unknowns-labeled` is preferred and was not the reason for the verdict.

The only required failure was `thread-convention: fail`, with the evidence "Repo policy requires AI usage disclosed with tool and extent for any form of contribution; plan comment contains no such disclosure." The repo-facts line says "All AI usage in any form must be disclosed, stating the tool used and the extent of the assistance". That matches the policy half of my check. The comment follows the thread's direction, so the thread half would have passed on its own. My verdict rule is "Accept only if every required check passes", so that one required failure rejects a plan that is otherwise ready.

**Check rationale**

`thread-convention`, quoted from the uploaded `tools/plan-check/rubric.md`:

```
| thread-convention | The candidate plan comment, read against the Thread highlights and the repo-facts contribution-policy line. | Pass the thread half when no OWNER, MEMBER, COLLABORATOR, or CONTRIBUTOR highlight names a file, function, constant, or approach to use or to avoid, and none posts a patch or binary and asks for it to be tested. When such a direction is present, pass only if the comment names that artifact and says it is following it, narrowing it, or deliberately not following it. A comment that only restates a different plan and never names the directed file, function, approach, or test request fails. Pass the policy half when the policy does not require disclosing AI use in issue comments. "No stated AI policy", a responsibility rule ("you must understand your code"), "low-quality AI content is closed", "comments must be written by a human", "assistive AI is allowed", and "no disclosure ask for issue comments" are not disclosure requirements, and a pull-request-only rule does not apply to these comments. Fail the policy half only when the policy says AI usage must be disclosed, or must state the tool and the extent, for issues, comments, or any form of contribution, and the plan comment contains no explicit disclosure (AI was used, plus the tool or the extent). These packages are AI-assisted work: human-sounding prose is not a disclosure, and silence fails that requirement. Both halves must pass. | required |
```

I wrote this as one required check because the `thread-convention` category has only two packages, and they fail for different reasons: `pkg-04`'s comment never names the owner's file and test binary, and `pkg-20`'s comment never discloses AI use. A check that only looked for maintainer direction would accept `pkg-20`. A check that required every comment to disclose AI would reject clear accepts whose repos have no disclosure ask, including `pkg-03` ("comments must be written by humans") and `pkg-09` ("no disclosure ask for issue comments"). The list of non-requirements is the same distinction I used in unit 2, applied to the plan comment only. The sentence "These packages are AI-assisted work: human-sounding prose is not a disclosure" is there so a well-written comment with no disclosure sentence still fails `pkg-20`.

**Trade-offs**

`thread-convention` is required and ignores how good the plan is, so an excellent plan in a must-disclose repo is rejected for one missing sentence. `pkg-20` passed the other four required checks and was still rejected. I accept that because it matches the gold label.

The case I accept it will miss: a comment written with no AI help at all, in a repo like ghostty, still fails, because the check treats every package as AI-assisted and needs an explicit sentence. For my own comment on #72 this does not matter. I used Claude, so the posted plan says so, even though Path Review's `docs/CONTRIBUTING.md` states no AI policy.

The revision that did change a verdict was `cause-grounded`, not this check. Run 1 rejected `pkg-14` because the empty-cache control was read as ruling out a reattach-path cause. I added one sentence: a control the repro itself says fits, because that run took a different path, does not rule the cause out. Run 2 re-graded `pkg-14` together with every wrong-cause package (`pkg-01`, `pkg-07`, `pkg-11`, `pkg-16`) and both thread-convention packages (`pkg-04`, `pkg-20`). `pkg-14` accepted, the four wrong-cause packages stayed rejected, and both thread-convention packages stayed rejected (`wrong-cause 4/4`, `thread-convention 2/2`). I changed nothing after that canary, and the full run agreed on all 20.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
