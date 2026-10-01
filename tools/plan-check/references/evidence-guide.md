# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

Where it lives: in an eval bundle, the candidate plan's Diagnosis section or the paragraph that states the cause, read against the Repro evidence section (steps, control runs, timings, pasted artifacts). In live mode, the cause is in `plan.md`, and the reproduction is the student's posted repro comment on the issue, or the repro evidence quoted in the drafts when there is no posted repro.

What good looks like: the stated cause is the behavior the controls pin down. A control fails the cause only when the blamed component is still on the path that showed the symptom, or is off that path and the symptom remains. A control the repro says fits, because the run took a different path, does not fail it. A plan that says only "investigate" or "profile" has no cause.

## Scope

Where it lives: the candidate plan's Scope section, the In and Out lines, or the numbered proposed changes. Compare that list with the single symptom named in the issue body.

What good looks like: one change for that symptom, with a larger idea explicitly deferred. The same bug fixed at a second site in the same function is still one change. A dependency migration, a new settings panel, a subsystem rewrite, a cross-runtime abstraction, a CI redesign, or a retry framework, added on top of the direct fix, is not one change.

## Executability

Where it lives: the candidate plan's Files section, Approach, or Change paragraph.

What good looks like: a named file, module, or site, plus the edit to make there. The exact function may be left to a trace the plan already describes. "Somewhere", "whichever is easier", "optimize whatever turns up", and "not sure which layer" are not a startable edit.

## Test plan

Where it lives: the candidate plan's Test plan section or the Test line, read against the symptom and steps in the Repro evidence.

What good looks like: an observable result for that symptom, such as a specific output, an exit code, an assertion, or the repro step's expected after-state. A manual re-run of the repro counts. "Should feel fast", "nothing else should feel broken", and "run the full test suite" with no check of this bug do not.

## Honesty

Where it lives: risk, unknown, or deviation lines in the candidate plan. In live mode, also the `## Deviations` section of `plan.md` after a build.

What good looks like: an open measurement or an open choice is labeled as open or deferred. A deviation that says what changed and why is honest. Writing an unmade measurement as a settled result is not. This family never changes the verdict on its own.

## Comms

Where it lives: the candidate plan comment, read against the Thread highlights (the role in parentheses on each bullet) and the contribution-policy line in the repo-facts block. In live mode, the draft is `comment.md`, the thread is the issue's comments via `gh`, and the policy is the repo's CONTRIBUTING or AI policy file. Path Review's house rules say a classmate's plan does not block yours, and "same approach as above" is not a plan.

What good looks like: if an OWNER, MEMBER, COLLABORATOR, or CONTRIBUTOR names a file, function, constant, or approach, or posts a patch and asks for it to be tested, the comment names that artifact and says whether the plan follows it. If the policy says AI usage must be disclosed for issues, comments, or any form of contribution, the comment contains an explicit disclosure naming the tool or the extent. A policy that only requires human-written comments, understanding the code, or disclosure on pull requests does not require that sentence. These packages are AI-assisted work, so human-sounding prose is not a disclosure.
