# Procedure: how this skill grades a PR package

Execute these steps in order. Do not grade a check before its evidence has been gathered. In eval mode, do not fetch anything. In live mode, the evidence guide's live notes name the files and the command `git diff main...HEAD`.

## Read order

1. Read the plan context first. Write down the files it names, what it says is not in scope, any deferred deliverable, and each repro its test plan names.
2. Read the repo-facts pull-requests line and the contribution-policy line. Write down each concrete ask (issue reference, named sections, changelog or whatsnew, tests, AI disclosure). Ignore "no stated AI policy" as an ask.
3. Read the candidate PR title and description before the diff. Mark any sentence that claims exact fidelity, a full implementation, or no changes beyond the plan. Mark any deferral the description restates.
4. Read the diff after that, file by file. The description you just marked is what you will compare these files against.
5. Read the test evidence last, command by command, against the repro list from step 1.
6. Skim the commit subjects only after the diff. They do not override the diff.

## Evidence gathering

1. Plan fidelity. List every path in the diff. Beside each path, write "named", "in a named folder", "disclosed deviation", or "not in the plan". Quote one fidelity sentence from the description, or write "none". Quote a deferral only if both the plan and the description state it.
2. Test evidence. Copy the bug-repro command and the result stated with it. A control, a suite line, or a happy-path check is a note, not a second bug repro. If the test plan names two commands that each trigger the failure, copy both or write "missing" for the one that is absent. If there is no bug-repro command, write "narrative only".
3. Diff quality. Copy any debug print, commented-out block, dead function, leftover TODO, or hunk whose only change is reindent or identical reprint. If none, write "none".
4. Standards. Copy each concrete ask from repo facts and whether the description or the diff satisfies it. For AI, copy the policy sentence and either the disclosure sentence or "no disclosure".

## Check execution

1. Grade in this order: plan-fidelity, test-decisive, diff-reviewable, standards-met. Apply only that row's pass condition.
2. plan-fidelity passes when every diff path is named, in a named folder, or a disclosed deviation, and no fidelity claim is contradicted by an extra hunk or a missing undefferred deliverable. It fails on a silent extra file or on a claim the diff does not support.
3. test-decisive passes when the bug repro is shown with a specific result. A control or suite stated in one sentence does not fail it. It fails on narrative-only evidence, on a control substituted for the bug repro, or when a second bug-triggering command the test plan names is missing.
4. diff-reviewable passes when the debris list is "none". It fails when that list names a debug print, commented-out attempt, dead function, leftover TODO, or pure churn hunk.
5. standards-met passes when every concrete ask is satisfied under its pass condition, including the AI rule. It fails when a required reference, changelog file, or required disclosure is missing.
6. If the section a check needs is absent from the package, grade that check unclear. Do not fill the gap from the title's tone.

## Verdict assembly

1. Apply the rubric's verdict rule to the grades just recorded.
2. Accept only if every required check is pass. Reject if any required check is fail or unclear.
3. In the summary, one line per check: its name, its grade, and the fact that decided it. When more than one required check fails, quote the first failing check in rubric order as the deciding check.
4. End with the fenced JSON block. The item is the bundle id or the PR URL. The verdict is accept or reject. Write nothing after that block.
