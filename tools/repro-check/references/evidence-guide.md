# Evidence guide: where proof lives in a reproduction package

The bundle is the whole world in eval mode. Use only these sections: the issue (title and body), the thread highlights, the repo-facts block, the candidate claim comment, and the candidate repro report. In live mode the same facts live on the issue page, the repo's CONTRIBUTING or AI policy, and the draft comments. Do not grade formatting. Grade what the artifact shows.

## Environment

Where it lives: a written environment statement in the repro report (a line or a small table naming versions). Compare it to the issue body, where the reporter names the version, OS, driver, shell, or build profile, and to the latest release in repo facts.

What good looks like: the report states the tool version and the OS, and also states any driver, shell, or build profile the issue says changes the failure. If those facts differ from the issue's confirmed target, the report says so in words. "Latest" or "main" in the issue means the latest release in repo facts or a current main build, not whatever older version happened to be installed. A major-version gap that is never mentioned is not a sufficient record. A Windows `route` line, or a panic that only debug builds emit, does not substitute for the missing statement.

## Steps

Where it lives: the numbered steps or the fenced commands in the repro report. The trigger they must hit is in the issue body: the command, the input document, the expression, and the flags the reporter used.

What good looks like: someone who is not the author can run that trigger from the text alone. The input matches the issue's input. An equivalent invocation passes when it still hits the same trigger and the report says what it changed (offline instead of a live POST, a newer build of the same command). It fails when the steps use a different range syntax, a different operator, or a different binding, when they drop a flag the issue's steps include, or when they point at a private repo or an unshared config.

## Behavior shown

Where it lives: fenced output, log excerpts, console text, and measurements inside the repro report. The symptom they must show is the one the issue names (the error string, the missing header, the wrong number, the panic, the crash, the blank pane), not a nearby failure.

What good looks like: the pasted text contains that symptom. These are not that symptom: exit 1 from argument validation when the issue is an exit 101 panic; `compile error` on an unbound name when the issue is `Invalid path expression`; an HCL message that asks for `=` when the issue is `panic: not a string`; `zellij --version` and a session list when the issue is a pane that never draws; pages of sixel payload with the window still open when the issue is a crash. A paragraph that names a root cause and says "I verified it", with nothing pasted, is not an artifact.

An evidenced cannot-reproduce is good too. The report says the bug did not appear, pastes what did appear, and names the difference (shell, OS, argument-length distribution, a limit that could not be lowered). That package is showing its work.

## Honesty

Where it lives: the report's Expected / Actual / Result sentences, and the claim comment's "I reproduced this" sentence, set next to the artifact above. The thread can tell you the maintainer's already-known boundary (which build, which flag).

What good looks like: the sentence and the artifact name the same event. "Cannot reproduce" plus a log of the healthy run is honest. "The crash is confirmed" plus a usage error, a still-running process, or a version banner is not. Expected and actual have to match the paste: if the paste shows the tabs present and the pane not shown to be blank, the report cannot say the blank pane was reproduced.

## Comms

Where it lives: the candidate claim comment, read against the issue title and body, and the repo-facts contribution policy. In live mode, the same policy is the repo's CONTRIBUTING.md or AI policy file.

What good looks like: the claim names the symptom or the next concrete step for this issue (a function, a file, an experiment). A one-line comment that does that is enough. "+1 also seeing this" with no next step is not, and neither is "assign this to me, I will fix it within 2 days guaranteed" with the bug unnamed.

Disclosure is a separate fact, decided only from the policy sentence in repo facts. If that sentence says AI use must be disclosed, or must name the tool and the extent, for comments, issues, or any contribution, one of the two comments has to say that AI was used and how. Sounding human is not that sentence. If the policy is silent, asks only that comments be the author's own words, limits disclosure to pull requests, or only says the author is responsible for the code, no disclosure sentence is required.
