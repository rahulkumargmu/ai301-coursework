# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72

**Verdict output**

Live-mode run of the installed `issue-select` skill on three Path Review candidates. The skill ranked #72 first and recorded `accept` for it.

```
All three candidates are from the scoped repo (`codepath/pathreview-ai301-fa26-s3`, not archived). Repo-level facts apply to all: most recent default-branch commit 2026-09-16 (2 days ago, human author), last push 2026-09-16 (both well within 90 days), no `CONTRIBUTING.md`/`AGENTS.md`/`AI_POLICY.md` found (silent policy). No comments, assignees, or linked PRs on any of the three issues.

**Issue #73** — README/.env.example disagree on LLM API key var
- maintainer-active: pass — commit 2026-09-16, human author
- repo-in-use: pass — push 2026-09-16
- scope-fits: pass — one bounded doc/config sync, named files, 1–2h estimate
- unclaimed: pass — no assignee, no comments, no linked PR
- ai-policy: pass — no policy file found (silence passes)
- good-first-issue-signal (preferred): pass — labeled `good first issue`
- **Verdict: accept**

**Issue #72** — `verify_password` raises `UnknownHashError` instead of returning False
- maintainer-active: pass
- repo-in-use: pass
- scope-fits: pass — named bug, named files (`core/security.py`, `tests/unit/test_security.py`), xfail test to un-mark, 1–2h
- unclaimed: pass
- ai-policy: pass
- good-first-issue-signal: pass — labeled `good first issue`
- **Verdict: accept**

**Issue #68** — Keyword search raises `ZeroDivisionError` on empty index
- maintainer-active: pass
- repo-in-use: pass
- scope-fits: pass — named bug, named files (`rag/retriever/keyword_search.py`, test file), xfail test to un-mark, 2–4h
- unclaimed: pass
- ai-policy: pass
- good-first-issue-signal: pass — labeled `good first issue`
- **Verdict: accept**

**Ranking (fit: Python backend/tests/docs, wants test-suite reading practice, avoids frontend/DevOps/tier-3):**
1. **#72** — matches best: `core/security.py` is exactly the FastAPI-style backend code you're comfortable in, shortest estimate (1–2h), and fixing an `xfail` test is direct practice reading/modifying an existing test suite.
2. **#68** — same test-suite-practice shape (un-xfail a test) but a less familiar domain (RAG/BM25) and a longer estimate (2–4h).
3. **#73** — safest and fastest, but it's pure docs/config reconciliation with no test suite involved, so it does less for your stated growth goal.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72",
    "checks": [
      {"name": "maintainer-active", "grade": "pass", "evidence": "default-branch commit 2f4e82f dated 2026-09-16 by human author Aburke225, repo not archived"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "repo pushed_at 2026-09-16T21:50:20Z, within 90 days of 2026-09-18"},
      {"name": "scope-fits", "grade": "pass", "evidence": "named bug in core/security.py with named test file, xfail marker H-05 to remove, 1-2h estimate"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: [], no comments, no linked PRs found via timeline/GraphQL"},
      {"name": "ai-policy", "grade": "pass", "evidence": "no CONTRIBUTING.md/AGENTS.md/AI_POLICY.md found (404) — silent policy"},
      {"name": "good-first-issue-signal", "grade": "pass", "evidence": "labels include 'good first issue'"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68",
    "checks": [
      {"name": "maintainer-active", "grade": "pass", "evidence": "default-branch commit 2f4e82f dated 2026-09-16 by human author Aburke225, repo not archived"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "repo pushed_at 2026-09-16T21:50:20Z, within 90 days of 2026-09-18"},
      {"name": "scope-fits", "grade": "pass", "evidence": "named bug in rag/retriever/keyword_search.py with named test file, xfail marker H-01 to remove, 2-4h estimate"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: [], no comments, no linked PRs found via timeline/GraphQL"},
      {"name": "ai-policy", "grade": "pass", "evidence": "no CONTRIBUTING.md/AGENTS.md/AI_POLICY.md found (404) — silent policy"},
      {"name": "good-first-issue-signal", "grade": "pass", "evidence": "labels include 'good first issue'"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73",
    "checks": [
      {"name": "maintainer-active", "grade": "pass", "evidence": "default-branch commit 2f4e82f dated 2026-09-16 by human author Aburke225, repo not archived"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "repo pushed_at 2026-09-16T21:50:20Z, within 90 days of 2026-09-18"},
      {"name": "scope-fits", "grade": "pass", "evidence": "named docs/config mismatch between README.md and .env.example, 1-2h estimate"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: [], no comments, no linked PRs found via timeline/GraphQL"},
      {"name": "ai-policy", "grade": "pass", "evidence": "no CONTRIBUTING.md/AGENTS.md/AI_POLICY.md found (404) — silent policy"},
      {"name": "good-first-issue-signal", "grade": "pass", "evidence": "labels include 'good first issue'"}
    ],
    "verdict": "accept"
  }
]
```
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. First full 20-issue attempt, before Claude auth was refreshed: `agreement: 0/0 scored items` and `20 item(s) errored; fix and re-run.` Every row was `ERROR` / `claude exited 1`.
2. Smoke run after login: `python3 run_eval.py --rubric ~/.claude/skills/issue-select/rubric.md --limit 3` → `agreement: 2/3 scored items`. `issue-01  accept  reject   NO     failed: scope-fits, good-first-issue-signal (preferred)`.
3. Confirming full run that wrote the committed file: `python3 run_eval.py --rubric ~/.claude/skills/issue-select/rubric.md --save-run eval-run.txt` → `agreement: 18/20 scored items  (bar: 18/20: PASS)` and `categories: claimed 4/4  clear-accept 6/8  dead-repo 3/3  policy 1/1  scope 4/4`. The last score matches the agreement line in `eval-run.txt`.

**Issue analysis**

`issue-01` (conda/conda#16475). Gold label: `accept` (`"docs task with a stated home and scope; active repo, unclaimed"`). My rubric's verdict: `reject`. The required check that failed was `scope-fits` (`failed: scope-fits, good-first-issue-signal (preferred)`). The preferred miss did not decide the verdict. `scope-fits` asks for "one bounded change" and fails a "list of many independent sub-items meant to be split." The bundle body names several docs homes (`new-features.md`, a new task page, `manage-pkgs.rst`, `pip-interoperability.rst`). The skill treated that as several pieces of work rather than one docs task with a stated home, so every required check did not pass and the verdict rule rejected it.

**Check rationale**

`scope-fits`, quoted from the uploaded `tools/issue-select/rubric.md`:

> Pass if the work is one bounded change a newcomer could ship from the text: a named bug/behavior, a named docs home, or a small feature a maintainer has already specified. A short body still passes when the asked-for change is bounded (including a short maintainer-filed bug). Fail if any of: (1) the issue is a megaissue, tracking issue, umbrella, or a list of many independent sub-items meant to be split; (2) the work is codebase-wide or "incrementally across the codebase" rather than one change; (3) the thread still has an unsettled design debate (maintainers disagree, or no MEMBER/OWNER/COLLABORATOR comment has chosen an approach) or the issue has been open for years with closed unmerged PRs and no settled spec; (4) the ask is a product decision still open in the text (new first-class branding/UI with asset TBD, no maintainer sign-off); (5) it is a usage/support question, not a contribution; (6) a maintainer says the fix touches core internals. Do not grade file counts; bundles usually do not list them.

I wrote it this way so the skill grades the size of the *work*, not polish or a file count the bundles do not include. Clause (1) and (2) are what reject `issue-05` (codebase-wide type-annotation umbrella) and `issue-10` (self-described megaissue). Clause (3) and (4) are what reject long design debates and product-decision wishes (`issue-15`, `issue-20`).

**Trade-offs**

The same `scope-fits` wording is what changed `issue-01` and `issue-19` from gold `accept` to my `reject`. I am accepting those two misses: a docs task that names several pages, and a maintainer-filed UI freeze that lists two causes, both look like "a list of sub-items" under clause (1). I did not re-run `--only issue-01,issue-19` after the passing full run, because the bar already read `agreement: 18/20 scored items  (bar: 18/20: PASS)` and loosening the list clause would risk the `scope` category (`issue-05`, `issue-10`).

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. `#72` is a bounded Python bug in `core/security.py` with an existing `xfail` test. That matches the time I have this week (the issue estimates 1–2 hours) and the backend/test work I want to practice. I passed on `#73` because it is docs-only and on `#68` because it is longer and more RAG-specific.
2. The verdict correctly called the repo alive, the work one named change, and the issue unclaimed, and it recorded `accept`. What I weighed that the rubric cannot: Path Review classmates may also take it, and that is allowed by the house rule; I still picked `#72` because I can read the test and the hash-handling code without a product decision.
3. Claiming it should be easy technically, but other students can comment on the same issue. Unit 2 is when I write the claim comment. I am not commenting now.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
