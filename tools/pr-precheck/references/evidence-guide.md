# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

Where it lives in an eval bundle: the Plan context, especially the Files list, the "not in scope" sentences, and any "deliberately deferred" note; the candidate PR diff (each `--- a/` / `+++ b/` path); the description's sentences that claim the plan was implemented exactly or in full. A deviation the description discloses is in the description, and it counts only when the plan records it too.

Where it lives on your own PR: `plan.md` (including `## Deviations`), the output of `git diff main...HEAD`, and the Summary, Changes, and Notes in `pr_draft.md`.

What good looks like: every changed file is one the plan names, or sits in a folder the plan names for tests or a changelog entry. Work the plan defers is still good when the description repeats that deferral. Silent drift is a file or behavior the plan did not name and nobody disclosed, or a description that claims a deliverable the diff does not contain.

## Test evidence (harness category: not-tested)

Where it lives in an eval bundle: the plan's Test plan sentences, then the candidate PR's Test evidence section. The issue's repro steps say which command is the real bug and which command is a control.

Where it lives on your own PR: the Test plan in `plan.md`, and `test_evidence.md` (the before run, the after run, and `make test-unit`, `make test-integration`, `make lint`, `make typecheck`).

What good looks like: the bug repro the test plan says to re-run appears as a command, with a specific result next to it (printed output, an exit code, or a before/after, including a result written after `#` or `--`). A control, a suite run, or a happy path may be one sentence once that bug repro is shown. "Tests pass" or "works on my machine", with no bug-repro command, is not evidence. Running only the control that never hit the bug is not evidence. When the test plan names two commands that each trigger the failure, both have to appear. A check that failed, pasted with the reason, is evidence.

## Diff quality (harness category: unreviewable)

Where it lives in an eval bundle: the unified diff under the candidate PR, then the commit list. Read the hunks, not the commit subjects, for debris.

Where it lives on your own PR: `git diff main...HEAD`. `git diff main...HEAD --stat` is the file list a reviewer sees.

What good looks like: the fix is visible and nothing unrelated is buried in it. Debris is a debug print left in, a commented-out first attempt, a dead function, a TODO on a leftover line, or a hunk that only reindents or reprints the same lines. An extra behavior the plan did not ask for is plan fidelity, above, unless it is also debris.

## Standards and comms (harness category: standards-wall)

Where it lives in an eval bundle: the repo-facts pull-requests line and the contribution-policy line. The description is where those asks are honored. A changelog or whatsnew file the template requires is honored only if that path appears in the diff.

Where it lives on your own PR: `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md` in Path Review, filled in inside `pr_draft.md`. The course disclosure goes in Notes for Reviewers, in your own words, because the template has no AI section.

What good looks like: a required `Closes #` or `fixes #` contains the issue number, each named template section has real content or a reasoned N/A, and a required changelog file is in the diff. When the policy says all AI use must be disclosed, the description names the tool or the extent. Silence passes only when the policy states no AI policy. A description that claims a section was done, while the diff lacks it, fails plan fidelity as well.
