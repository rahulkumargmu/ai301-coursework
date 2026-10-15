# issue-scout scan output

## Scan 1: 2026-10-14 (claude -p "issue-scout: run a scan" --model sonnet, cost $1.02, 42 turns)

## Scan Results — Top 5 Candidates

**Queries run:** 3 (good-first-issue+bug/Python, good-first-issue+documentation/Python, good-first-issue+fastapi/Python), each `--sort updated`, `--created "<2026-09-23"`. Combined 293 unique results → 69 after full ranking filter chain (assignee/codepath/OWNER-author/bad-label/comment-count/date-window drops, one-per-repo cap) → ordered by fewest comments then most recent update.

### Accepted (2), ranked by fit

**1. [workfloworchestrator/orchestrator-core#1872](https://github.com/workfloworchestrator/orchestrator-core/issues/1872)** — "ALL_SCHEDULERS deprecation warning fires on any import"
Surfaced by: query 1 (bug+gfi). Ranking: 0 comments, updated today, repo cap winner for this org.
Why it fits: textbook bounded bug — exact repro command, root cause, and suggested fix (`__getattr__` + stale version string) spelled out by the issue opener; extremely active repo (commits today, release cut today); no AI-contribution restriction found.

**2. [TiBillet/Lespass#450](https://github.com/TiBillet/Lespass/issues/450)** — "Quand une adhésion est annulée, l'affichage de la date d'expiration n'est pas clair"
Surfaced by: query 1 (bug+gfi). Ranking: 0 comments, updated 2026-10-12.
Why it fits: a bounded display bug with a screenshot and clear repro steps (cancel a membership, see the stale deadline rendered); repo pushed to today. Note: issue text is in French — worth confirming you're comfortable working from that before claiming it.

### Rejected (3)

**3. [aeon-toolkit/aeon-neuro#78](https://github.com/aeon-toolkit/aeon-neuro/issues/78)** — "[DOC] Add examples to the transformers"
Surfaced by: query 2 (documentation+gfi). Sunk by: `scope-fits` — a 14-item checklist spanning multiple transformer files, explicitly scoped as "one PR per group" — a tracking/umbrella issue meant to be split, not one bounded change.

**4. [open-nvr/open-nvr#29](https://github.com/open-nvr/open-nvr/issues/29)** — "Users are unable to view the live camera feed without installing the SSL CA certificate"
Surfaced by: query 1 (bug+gfi). Sunk by: `scope-fits` — issue body is completely empty; title alone doesn't establish what bounded change is being asked for (docs fix? cert-handling code change? UX note?), graded unclear → treated as fail.

**5. [mila-iqia/cluv#200](https://github.com/mila-iqia/cluv/issues/200)** — "`cluv submit mila` from laptop shouldn't pull dataset and push it back"
Surfaced by: query 1 (bug+gfi). Sunk by: `scope-fits` — the opener (a CONTRIBUTOR, not a maintainer) floats a possible fix and immediately second-guesses it ("feels dumb"); zero comments, so no MEMBER/OWNER/COLLABORATOR has settled on an approach — unsettled design, not a newcomer-ready bounded task.

```json
[
  {
    "item": "https://github.com/workfloworchestrator/orchestrator-core/issues/1872",
    "checks": [
      {"name": "maintainer-active", "grade": "pass", "evidence": "Repo not archived; multiple human-authored default-branch commits today (2026-10-14), e.g. Mark Moes, timfdev, tjeerddie"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "Last push today (2026-10-14T15:16:45Z) and release 5.5.0 published today"},
      {"name": "scope-fits", "grade": "pass", "evidence": "Body gives exact repro command, root cause (module-scope warning), and the proposed fix (__getattr__ + version string correction)"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: [], comments: [], no linked PRs via timeline query"},
      {"name": "ai-policy", "grade": "pass", "evidence": "No CONTRIBUTING.md found in repo root or .github/ — silent policy"},
      {"name": "good-first-issue-signal", "grade": "pass", "evidence": "Labels: ['bug', 'good first issue']"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/TiBillet/Lespass/issues/450",
    "checks": [
      {"name": "maintainer-active", "grade": "pass", "evidence": "Repo not archived; commits by NothRen today (2026-10-14T18:50:35Z)"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "Last push today (2026-10-14T18:50:39Z); no releases published but recent push/commit satisfies the OR condition"},
      {"name": "scope-fits", "grade": "pass", "evidence": "Body (French) gives a clear repro — cancel a membership via admin, deadline display stays wrong — with a screenshot attached"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: [], comments: [], no linked PRs via timeline query"},
      {"name": "ai-policy", "grade": "pass", "evidence": "No CONTRIBUTING.md found at root or .github/ — silent policy"},
      {"name": "good-first-issue-signal", "grade": "pass", "evidence": "Labels: ['bug', 'good first issue']"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/aeon-toolkit/aeon-neuro/issues/78",
    "checks": [
      {"name": "maintainer-active", "grade": "pass", "evidence": "Commits today (2026-10-12) by Harkamal S, Tony Bagnall, CRYPTONIKAV"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "Last push 2026-10-12; release v0.0.2 from 2025-04-21 is outside 365d but recent push/commits satisfy the OR"},
      {"name": "scope-fits", "grade": "fail", "evidence": "Checklist of 14 separate transformer classes across multiple files, explicitly noted as splittable (\"One PR per group of related transformers is fine\") — a tracking/umbrella issue"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: [], comments: []"},
      {"name": "ai-policy", "grade": "pass", "evidence": "No CONTRIBUTING.md found in repo root"},
      {"name": "good-first-issue-signal", "grade": "pass", "evidence": "Labels: ['documentation', 'good first issue']"}
    ],
    "verdict": "reject"
  },
  {
    "item": "https://github.com/open-nvr/open-nvr/issues/29",
    "checks": [
      {"name": "maintainer-active", "grade": "pass", "evidence": "Commits today (2026-10-14) by sipworld, xpertvoip"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "Last push today; release v0.1.7 published today"},
      {"name": "scope-fits", "grade": "unclear", "evidence": "Issue body is empty string (\"\") — only the title exists, no reproduction or specified fix direction"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: [], comments: [], no linked PRs via timeline query"},
      {"name": "ai-policy", "grade": "pass", "evidence": "CONTRIBUTING.md exists but sets only workflow conditions (fork, tests, PR flow); no AI-generated-code restriction"},
      {"name": "good-first-issue-signal", "grade": "pass", "evidence": "Labels: ['bug', 'good first issue']"}
    ],
    "verdict": "reject"
  },
  {
    "item": "https://github.com/mila-iqia/cluv/issues/200",
    "checks": [
      {"name": "maintainer-active", "grade": "pass", "evidence": "Commits today (2026-10-14) by hvdbm, Wietze, Fabrice Normandin"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "Last push today; release v0.1.5 published 2026-10-01"},
      {"name": "scope-fits", "grade": "fail", "evidence": "Opener (authorAssociation: CONTRIBUTOR) proposes a flag then says \"this feels dumb\"; 0 comments, so no MEMBER/OWNER/COLLABORATOR has settled on an approach"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: [], comments: [], no linked PRs via timeline query"},
      {"name": "ai-policy", "grade": "pass", "evidence": "CONTRIBUTING.md exists but only covers dev setup/testing/linting; no AI restriction"},
      {"name": "good-first-issue-signal", "grade": "pass", "evidence": "Labels: ['bug', 'good first issue']"}
    ],
    "verdict": "reject"
  }
]
```
