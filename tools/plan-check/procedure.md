# Procedure: how this skill grades a plan package

Execute these steps in order. Do not grade a check before the read order and the evidence gathering for that check are done. Do not fetch anything in eval mode. In live mode, gather issue-side evidence the way the evidence guide's live notes say, and take the reproduction from the posted repro comment or from the repro evidence the drafts quote.

## Read order

1. Read the Repro evidence section first. Note the symptom, the steps, every control run, and any timing or pasted artifact. Write down what the controls change and what they leave unchanged.
2. Read the Issue title and body only to name the reported symptom. Do not take a cause from the issue body if the repro's controls contradict it.
3. Read the Thread highlights. For each comment, note the role in parentheses and whether it names a file, function, constant, approach, or a patch or binary to test.
4. Read the repo-facts contribution-policy line. Note whether it requires disclosing AI use in issue comments, using the exceptions in the thread-convention check.
5. Read the candidate plan: the stated cause, the scope or proposed changes, the files and approach, the test plan, and any risk or unknown line.
6. Read the candidate plan comment last. A thread comment that agrees with the plan is not proof the cause is right. Grade the cause against the Repro evidence, not against the thread.

## Evidence gathering

1. Diagnosis and grounding: copy the plan's stated cause into one sentence. Beside it, copy the repro behavior the controls actually pin down, including a control that rules a component out. Compare those two sentences.
2. Scope: list each change the plan says it will make. Mark any larger idea it explicitly defers or puts out of scope. Compare the will-make list with the one symptom from the issue.
3. Executability: copy the file, module, or site the plan names, and the edit it describes there. If either is missing, record "missing".
4. Test plan: copy the result the test says a stranger would observe, and the repro symptom it is supposed to show is gone.
5. Honesty: copy any sentence that says a measurement or decision is still open, and any sentence that states that same point as already done.
6. Comms: copy any OWNER, MEMBER, COLLABORATOR, or CONTRIBUTOR direction that names a file, function, constant, approach, or a test of a posted patch. Copy whether the plan comment names that same artifact. Copy the policy sentence and whether the plan comment contains an explicit AI disclosure (the tool or the extent). Human-sounding prose is not a disclosure.

## Check execution

1. Grade the five required checks in this order: cause-grounded, scope-bounded, executable, test-decisive, thread-convention. Then grade unknowns-labeled.
2. Apply only that row's pass condition. A check that meets its condition passes even if the plan feels wrong for another reason.
3. cause-grounded is P only when the stated cause matches the repro's controls. It is F when a control contradicts the cause, using the rubric's rule for when a control rules a cause out, or when no cause is stated. A control the repro says fits because it took a different path is not a contradiction.
4. scope-bounded is P only when every change the plan will make serves the one symptom, with larger work explicitly deferred. It is F when an independent migration, panel, rewrite, abstraction, CI redesign, or retry framework is in the will-make list.
5. executable is P only when a file or site and a concrete edit are both present. It is F when the plan stops at investigate, profile, somewhere, or whichever is easier.
6. test-decisive is P only when the test names an observable result for the repro's symptom. It is F when the test is a feeling, a full-suite run with no symptom check, or a nearby fact.
7. thread-convention is P only when both halves of its pass condition hold. It is F when a named direction is ignored, or when a must-disclose policy has no explicit disclosure in the plan comment.
8. unknowns-labeled is P when open points are labeled open, or when the plan states no uncertain claim. It is F when an open point is written as settled. Its grade never changes the verdict.
9. If the part a check needs is missing from the package, grade that check unclear. Do not invent the missing fact from the issue title or from a confident tone.

## Verdict assembly

1. Apply the rubric's verdict rule to the grades just recorded.
2. Accept only if every required check is pass. Reject if any required check is fail or unclear.
3. Ignore unknowns-labeled when choosing the verdict.
4. In the summary, one line per check: its name, its grade, and the fact that decided it.
5. End with the fenced JSON block. The verdict field is accept or reject. Write nothing after that block.
