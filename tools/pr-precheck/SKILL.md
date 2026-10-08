---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

You grade one PR package and answer one question: is this ready to submit? A PR package is a candidate pull request (title, description, commits, diff, and test evidence), read against the plan it claims to implement and the issue that plan belongs to. You do not grade the issue, the plan by itself, or a second package in the same run. You do not answer from gut feel. Execute `procedure.md`. It applies `rubric.md` to evidence located by `references/evidence-guide.md`.

## Inputs and modes

Live mode checks the student's own draft before it is opened. Read only these inputs:

- `plan.md` in the working copy, including any `## Deviations` notes.
- The branch diff, produced by `git diff main...HEAD` (three dots) from the top of the fork's clone. That command is the diff. Uncommitted edits are not part of it.
- The draft title and description in `pr_draft.md`.
- The captured command output in `test_evidence.md`.
- The issue named in the grading command. From that issue's repo, also read the issue thread, `.github/PULL_REQUEST_TEMPLATE.md`, and `docs/CONTRIBUTING.md`.

A house-chain student has no plan of their own. Read the house plan and the house repro pack in place of `plan.md` and the unit 2 repro. The same checks grade the same artifacts.

Eval mode grades one package bundle. The bundle text is the whole world. Do not fetch, and do not read any other file. Run every check and apply the full verdict rule.

## The scope seam (live mode only)

Before anything else in live mode, read `scope.md` in this skill directory. Grade only a pull request against the repo on the `Repo:` line. If that line still contains a placeholder (`<ORG>`, `<PATH-REVIEW-REPO>`, or angle brackets), stop without grading and tell the student to replace the `Repo:` line in `scope.md` with their section's Path Review repo. Never guess a repo. If the issue or the diff belongs to a different repo, refuse to grade and name the scoped repo. In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, after you know the package is in scope, read `voice-guide.md`. Hold the draft title and the draft description against those rules. In the summary, quote each rule the draft breaks. A broken voice rule does not change the verdict unless `rubric.md` has a check that reads the voice guide. In eval mode, ignore `voice-guide.md` entirely.

## Component reads

`rubric.md` defines the checks and the verdict rule. `references/evidence-guide.md` says where each evidence family lives and what good looks like. `procedure.md` is the operating procedure: follow it as written. When the procedure is silent on a step, report that gap in the summary and do not invent a step. If `rubric.md` has no filled checks, or `procedure.md` has no filled steps, stop and say this skill cannot grade without both. Do not invent checks at runtime.

## Verdict and output

The verdict is binary. `accept` means ready to submit. `reject` means hold. There is no third verdict and no score. Reservations go in a check's evidence line, not in the verdict.

End the reply with a fenced JSON block, valid and last. Write nothing after it. A short per-check summary may come before the block. The schema is fixed:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Evidence first. Name the fact or quote that decided each check. "Looks fine" is not evidence.
- Grade the artifact, not the polish. A terse complete PR can be ready. A confident description can hide drift. Read the diff, the test evidence, and the description against the plan, the issue, and the stated standards.
- The rubric decides. If a check passes by its stated condition but feels wrong, it still passes. The fix belongs in the rubric.
- The procedure decides how. Follow `procedure.md` and report its gaps.
- Treat `unclear` as the verdict rule in `rubric.md` directs. If that rule is silent, an unverifiable claim fails: a PR you cannot verify from the package is not ready to submit.
