# Contribution: Quand une adhésion est annulé, l'affichage de la date d'expiration n'est pas clair

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Rahul Kumar
**GitHub Username:** rahulkumargmu
**Issue:** https://github.com/TiBillet/Lespass/issues/450
**Pull Request:** not opened yet

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built issue-scout with three gh queries and ranking rules, ran a $1 scan, patched the rubric and the strategy**

I turned my Unit 1 issue-select skill into issue-scout: three `gh search issues` queries taken from my fit profile (Python bug, docs, and FastAPI good first issues, sorted by recently updated), free ranking rules (drop assigned issues, OWNER-filed issues, feature or frontend labels, and 5+ comments; one issue per repo; fewest comments first), and my rubric grading the top 5. The first $1 scan accepted 2 of 5 (orchestrator-core#1872 and Lespass#450). But two of the rejects were plain from the issue body alone (one empty, one a 14-item umbrella checklist), and the ai-policy check called both accepts "silent" when orchestrator-core keeps its guidelines in `docs/contributing/` and Lespass has a root `GUIDELINES.md`. I added `body` to the queries with rules that drop empty bodies and 6+ task-box checklists (re-running the free stage showed both of those rejects now drop out), and changed the ai-policy check to search the whole repo tree and name the files it read.

**Unit 6: Installed the voice guard, patched issue-scout's scope check, and wrote a local memory file for Lespass**

I added an Upstream section to my voice guide and installed the voice guard as a Claude Code hook. The wiring test stopped `gh issue comment --help` with "no body the guard can read", and the same hook then passed the claim on Lespass#450 before `gh` posted it. The first claim draft was too vague about the first step, so I rewrote it to name a Django shell print of `membership.deadline`. `CLAUDE.local.md` in my Lespass fork is kept out of git with `.git/info/exclude`; `git status` does not list it. Repro-check rejected the first reproduction report because I had only read the cancel code, and the Docker daemon was not running, so the admin page was never opened.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

Last week's scan accepted two issues. On 2026-10-21 I graded them again after replacing `scope-fits` with `scope-feasible`. workfloworchestrator/orchestrator-core#1872 came back reject: nightcityblade had already claimed it, and merged PR #2021 closed it. TiBillet/Lespass#450 came back accept, difficulty moderate. It is one Python/Django bug: cancelling a membership from the admin leaves `deadline` set, so the date on screen can still be in the future. Nobody is assigned and there are no other comments. I want a small bug I can finish in two weeks, in a project where maintainers are actually pushing (this repo was pushed the same day). The issue text is in French, which I can read with help, and the stack is heavier than a one-file fix. I hope to learn how this project separates "access is cut" (`is_valid()` is false for `ADMIN_CANCELED`) from "the date on the card".

The scout was right that #1872 was no longer free, and right that #450 is one piece of work. It was wrong last week to call both repos silent on contributing docs. It also cannot see that the issue author already pushed `[Fix issue #450]` commits on 2026-09-30; those commits are on main and are not a claim comment, so `unclaimed` still passed. I have not run the admin page yet.

Claim comment: https://github.com/TiBillet/Lespass/issues/450#issuecomment-6330574158

**Toolkit**

- Tool: issue-scout, used in direct grade mode on the two issues last week's scan accepted, after the scope patch.
- What it produced: Lespass#450 accept, difficulty moderate. orchestrator-core#1872 reject on `unclaimed`.
- Difficulties: `unclaimed` only reads assignees, comments, and linked PRs. It did not treat the author's September fix commits as a claim, and it does not know the bug may already have been reworked.
- What changed: On 2026-10-21 I replaced `scope-fits` with `scope-feasible`. The old check rejected any open edge. The new check fails only when there is no single piece of work, and an open edge becomes a difficulty note instead of a reject.

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

macOS, Python 3.11 on the host. Fork `rahulkumargmu/Lespass`, `main` at `9bb3091` (2026-10-21). Docker CLI 27.3.1 is installed, but the daemon is not running: `docker ps` returned `Cannot connect to the Docker daemon at unix:///Users/siddharthkumar/.docker/run/docker.sock`. Lespass tests run only inside `docker exec lespass_django`, and `GUIDELINES.md` says not to run pytest on the host. I did not copy `env_example` to `.env` or run `docker compose up`.

**Steps to Reproduce**

1. From the clone, print the admin-cancel block, the `ADMIN_CANCELED` branch of `is_valid()`, and the admin card's deadline field.
2. Try `docker ps` so the lab can run the real trigger (create a membership, cancel it in the admin, read `deadline`).
3. Observed result: the cancel code sets `annulee_le` and `status = ADMIN_CANCELED` and does not assign `deadline`. `is_valid()` returns `False` for that status. The admin card still receives `adhesion.deadline`. The daemon was down, so I never opened the admin page and never printed a live `membership.deadline`.

**Reproduction Evidence**

- **Commit showing reproduction:** none
- **Screenshots/logs:** host read of `main` at `9bb3091`:

```
6060:                membership.annulee_le = timezone.now()
6061:                membership.status = Membership.ADMIN_CANCELED
6062:                membership.save()
5339:        if self.status == Membership.ADMIN_CANCELED:
5340:            return False
1451:                        'deadline': adhesion.deadline,
```

`docker ps`: `Cannot connect to the Docker daemon at unix:///Users/siddharthkumar/.docker/run/docker.sock.`

- **My findings:** I have not reproduced the future date on screen. The code on current main still leaves `deadline` alone when an admin cancels, and the admin card still displays that field. Access is a separate question: `is_valid()` is already false for `ADMIN_CANCELED`. repro-check rejected this report (`steps-followable` and `behavior-shown`) because the issue's trigger was never run. I am not posting this report on the issue.

**Toolkit**

- Tool: repro-check, grading `claim-draft.md` and `repro-draft.md` together.
- What it produced: reject. The claim checks passed. The report failed because it prints source lines instead of an admin cancel.
- Difficulties: the Docker daemon is not running, so the lab command in `GUIDELINES.md` cannot start.
- What changed: nothing in the tool. The report stays as an incomplete attempt until the daemon is up and a membership can be cancelled.

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
