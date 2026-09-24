# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

rahulkumargmu

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5805528936

Before posting, I graded this draft with my installed `repro-check` skill in live mode as a claim-only draft. `claim-specific`, `ai-disclosure`, and `no-guaranteed-deadline` passed, the four repro checks reported `not yet applicable: claim-only draft`, and the verdict was `accept`.

Text as posted:

````markdown
I'd like to work on this as my first Path Review fix. Another student has also claimed it; following the course house rules, I'll do my own setup and reproduction and post my own report.

What the issue describes: `verify_password()` in `core/security.py` should return `False` when the stored hash is not a bcrypt hash, but passlib's `UnknownHashError` escapes instead. Reading the code, the function passes the stored hash straight to `pwd_context.verify()` with no error handling, which fits that description. I have not run anything yet.

What I'll do next:

1. Fork the repo, set it up with `docs/SETUP.md`, and record my OS, Python, passlib, and bcrypt versions and the commit I test.
2. Run `tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format` (the strict `xfail` for H-05) with `--runxfail` so the real exception is visible, and call `verify_password("password", "not_a_valid_bcrypt_hash")` directly, with a normal bcrypt hash as the control.
3. Try two more malformed values, an empty string and a truncated `$2b$` hash, to see whether they raise the same exception or a different one, since that decides what a fail-closed fix has to catch.

I'll post the commands and output here whether it reproduces or not. I used Claude (an AI assistant) to help draft this comment.
````

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5805585610

Before posting, I graded the full package (the posted claim plus this report) with `repro-check` in live mode. All seven checks passed and the verdict was `accept`.

Text as posted:

````markdown
Reproduction report for #72. **Result: reproduced** on current `main`. `verify_password()` raises `passlib.exc.UnknownHashError` for the test's malformed stored hash instead of returning `False`.

**Environment**

- macOS 26.5.2 (build 25F84), Apple Silicon (arm64)
- Python 3.11.10 (the version CI uses), fresh `.venv`
- passlib 1.7.4, bcrypt 4.3.0, pytest 9.1.1, all from `pip install -e ".[dev]"`
- Code: commit `2f4e82f`, the current `main` of `codepath/pathreview-ai301-fa26-s3`, checked out in my fork with no local changes
- Setup followed `docs/SETUP.md`: `cp .env.example .env`, `docker compose up -d` (Postgres and Redis healthy; `vector-db` exits, see the note at the end), then the `make setup` steps (venv, `pip install -e ".[dev]"`, pre-commit, `alembic upgrade head`, seed, frontend `npm install`), all of which succeeded. This bug only needs the Python part; no Docker service is involved.

**Steps**

```bash
git clone https://github.com/codepath/pathreview-ai301-fa26-s3.git
cd pathreview-ai301-fa26-s3 && git checkout 2f4e82f
python3.11 -m venv .venv && source .venv/bin/activate
pip install -e ".[dev]"

# 1. As shipped, the strict xfail hides the error
pytest tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format -v
# -> 1 xfailed, 2 warnings in 0.26s

# 2. The same test with the xfail marker ignored
pytest tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format -v --runxfail --tb=short
```

Output of step 2 (session header and the two unrelated deprecation warnings trimmed):

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
=========================== short test summary info ============================
FAILED tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format
======================== 1 failed, 2 warnings in 0.18s =========================
```

3. Direct calls in the same venv, with a real bcrypt hash as the control, plus the two extra malformed values from my claim:

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

Output (stdout):

```
control, right password -> True
control, wrong password -> False
'not_a_valid_bcrypt_hash'   -> passlib.exc.UnknownHashError: hash could not be identified
''                          -> passlib.exc.UnknownHashError: hash could not be identified
'$2b$12$abc'                -> builtins.ValueError: salt too small (bcrypt requires exactly 22 chars)
UnknownHashError is a ValueError: True
```

**Expected:** `verify_password()` returns `False` when the stored value is not a usable bcrypt hash, the same answer it gives for a wrong password.

**Actual:** for the test's `"not_a_valid_bcrypt_hash"`, and for an empty string, passlib's `UnknownHashError` escapes from `core/security.py:37`. Real hashes behave correctly (`True` for the right password, `False` for a wrong one). A truncated bcrypt-looking hash (`$2b$12$abc`) also raises, but as a plain `ValueError` ("salt too small"), not `UnknownHashError`. So a fix that catches only `UnknownHashError` would still let that input raise. `UnknownHashError` is itself a `ValueError`.

Not part of this bug, noted so nobody chases them: passlib 1.7.4 prints a trapped `AttributeError: module 'bcrypt' has no attribute '__about__'` traceback to stderr the first time it loads bcrypt 4.3.0 (left out of the step 3 output above). Separately, the `vector-db` container (`chromadb/chroma:0.4.22`) exits on start with ``AttributeError: `np.float_` was removed in the NumPy 2.0 release``. Neither affects this test, and the other 24 tests in `tests/unit/test_security.py` pass.

Next I'll work on a change to `verify_password()` that returns `False` for these inputs, remove the strict `xfail` marker, and link the PR here.

I used Claude (an AI assistant) to help run these steps and draft this comment. The commands ran on my machine, and the output above is copied from those runs.
````

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Partial check run on 15 scored packages chosen to cover every category, plus `calib-03` and `calib-04`: `python3 run_eval.py --rubric ~/.claude/skills/repro-check/rubric.md --evidence ~/.claude/skills/repro-check/references/evidence-guide.md --include-calibration --only pkg-01,pkg-02,pkg-03,pkg-05,pkg-06,pkg-07,pkg-09,pkg-10,pkg-11,pkg-12,pkg-14,pkg-16,pkg-17,pkg-19,pkg-20,calib-03,calib-04`. Result: `agreement: 15/15 scored items` and `categories: clear-accept 8/8  disclosure 1/1  no-evidence 1/1  unfollowable-comms 2/2  wrong-target 3/3`. Both calibration packages matched (`calib-03  reject  reject   yes`, `calib-04  reject  reject   yes`). It was a partial run, so it printed no bar verdict.
2. Confirming full run, with no rubric or evidence-guide changes after run 1: `python3 run_eval.py --rubric ~/.claude/skills/repro-check/rubric.md --evidence ~/.claude/skills/repro-check/references/evidence-guide.md --save-run eval-run.txt`. Result: `agreement: 20/20 scored items  (bar: 18/20: PASS)` and `categories: clear-accept 8/8  disclosure 1/1  no-evidence 4/4  unfollowable-comms 3/3  wrong-target 4/4`. This run wrote the committed `eval-run.txt`, and its agreement line matches.

**Package analysis**

`pkg-20` (ghostty-org/ghostty#13604, category `disclosure`). The gold label is `reject`: "excellent repro on every proof check; ghostty's stated AI policy requires disclosing all AI usage and the comments do not disclose (course packages are treated as AI-assisted work)". My rubric's verdict was also `reject` (`pkg-20  reject  reject   yes`).

My rubric passed the proof itself. The same run's `--out` results show `env-recorded: pass` ("ghostty 1.3.1 (release build, Fedora 42 RPM), GTK backend, GNOME 48 (Wayland), dark system scheme") and `behavior-shown: pass` ("Pasted `^[[?997;2n` for the single-theme run matches the issue's named wrong-response symptom exactly"). `steps-followable`, `outcome-honest`, `claim-specific`, and `no-guaranteed-deadline` passed as well.

The only failure was `ai-disclosure: fail`, with the evidence "Repo facts require disclosing AI usage (tool + extent) for 'any form' of contribution including issues/comments, but neither the claim comment nor the repro report contains any such disclosure." The repo-facts line says "All AI usage in any form must be disclosed, stating the tool used and the extent of the assistance". That matches my check's fail condition: "the policy says AI usage must be disclosed, or must state the tool and the extent, for issues, comments, or any form of contribution". Neither comment has a disclosure sentence. My verdict rule is "Accept only if every required check passes", so that one required failure rejects a package whose reproduction is otherwise strong.

`pkg-07` is the contrast. Its claim says "Per the AI usage policy: I used an AI assistant to help me organize this report", so `ai-disclosure` passed there and the package was accepted, matching gold.

**Check rationale**

`ai-disclosure`, quoted from the uploaded `tools/repro-check/rubric.md`:

```
| ai-disclosure | The repo-facts contribution policy, read against the claim comment and the repro report. | Pass when the policy does not require disclosing AI use in issue comments. "No stated AI policy", a responsibility rule ("you must understand your code"), "low-quality AI content is closed", "comments must be written by a human", and "no disclosure ask for issue comments" are not disclosure requirements, and a pull-request-only rule does not apply to these comments. Fail only when the policy says AI usage must be disclosed, or must state the tool and the extent, for issues, comments, or any form of contribution, and neither comment contains an explicit disclosure (AI was used, plus the tool or the extent). These packages are AI-assisted work: human-sounding prose is not a disclosure, and silence fails that requirement. A disclosure in either comment is enough. | required |
```

I wrote this check before my first run, after reading the repo-facts line in all 20 packages, and it did not change between the two runs. Here is what I rejected in its favour:

- **A general "respects the repo's conventions" check.** It has no observable test. A grader could fail `pkg-03` because ripgrep says "comments to maintainers must be written by humans in their own words", or pass `pkg-20` because its comments read as human-written.
- **"Every comment must disclose AI."** That would reject clear accepts like `pkg-01` and `pkg-11`, which have no disclosure line and whose repos have no AI policy.

So the check reads one fact, the policy sentence, and looks for one sentence in the comments. The list of non-requirements is there because four scored packages mention AI in their policy without asking for disclosure in comments:

- `pkg-03`: "comments to maintainers must be written by humans in their own words"
- `pkg-05`: "must review and understand AI-generated content before including it in a pull request"
- `pkg-09`: "the policy states no disclosure ask for issue comments"
- `pkg-12`: "low-quality AI content is closed immediately"

The sentence "These packages are AI-assisted work: human-sounding prose is not a disclosure" answers the `pkg-20` gold note, "course packages are treated as AI-assisted work". Without it, a grader can decide that well-written comments were not AI-assisted, find nothing to disclose, and pass the one package the disclosure category floor depends on.

**Trade-offs**

`ai-disclosure` is required and ignores proof quality, so a strong reproduction in a must-disclose repo is rejected for one missing sentence. `pkg-20` passed the other six checks and was still rejected. I accept that because it matches the gold label.

The case I accept it will miss: a comment written with no AI help at all, in a repo like ghostty, still fails, because the check treats every package as AI-assisted and needs an explicit sentence. For my own comments this does not matter. I used Claude, so both comments on #72 say so, even though Path Review's `docs/CONTRIBUTING.md` states no AI policy.

Here is how I know it did not change anything else. The check can only fail a package whose policy requires disclosure and whose comments do not disclose, and in this set that is only `pkg-20`. My first run included every scored package whose policy mentions AI (`pkg-03`, `pkg-05`, `pkg-07`, `pkg-09`, `pkg-12`, all gold `accept`) as canaries next to `pkg-20`. All five were accepted and `pkg-20` was rejected (`disclosure 1/1`). The full run then agreed on all 20 with `clear-accept 8/8`. I changed nothing after the full run, so there was no loosened check to re-run with `--only`.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
