# Search strategy: where your scout looks, and why

<!--
THIS IS THE PART YOU WRITE. The scout in SKILL.md executes whatever
queries and filters you define here. It ships empty on purpose: the
strategy is your judgment about where YOUR first contribution lives,
and a scan without authored queries is just someone else's defaults.

A filled strategy must contain:

1. At least one query in the Queries section. Each query is a real
   `gh search issues` invocation, and each should trace to a sentence
   of your fit profile in scope.md: if you cannot say which sentence
   produced a query, the query is probably not yours. Search breadth
   is the point at this stage: prefer generous limits and let the
   ranking filters do the narrowing.

2. At least one ranking filter in the Ranking filters section. A
   filter is a condition or an ordering over fields the search
   results already carry (last activity date, assignee presence,
   label set, comment count, repo). It must be checkable from the
   result fields alone: if deciding it would need opening the issue
   page, it belongs in your rubric, not here. Ranking is the free
   stage; keep it free. Make at least one filter an ordering, or
   state your tie-break: with drop conditions alone, the scout takes
   the top of whatever order the searches happened to return, and
   the graded candidates stop being traceable to you.

3. The top-N line, kept or deliberately changed.
-->

## Queries

<!-- Real gh search invocations, one per line, each with a comment
naming the fit-profile sentence it came from. Example SHAPE (write
your own; these values belong to nobody):

gh search issues --label "good first issue" --language python --state open --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,url
-->

Notes that apply to every query:
- `--sort updated` because, with `--limit 100`, the default order fills
  the whole page with issues opened in the last few days in brand-new
  personal repos. Recently updated issues that are a few weeks old or
  more come mostly from maintained projects.
- `--created "<2026-09-23"` drops issues opened in the last three
  weeks (the Hacktoberfest and practice-repo churn). It is a fixed
  date, so move it forward each week to about three weeks before the
  scan date.
- Negated filters go after `--`. Don't put `OR` there: gh wraps
  everything after `--` in parentheses, and that changes what `OR`
  groups.

# "I want a first issue that is one bounded Python bug ... not a new feature."
gh search issues --label "good first issue" --label bug --language python --state open --no-assignee --comments "<5" --created "<2026-09-23" --sort updated --limit 100 --json title,url,repository,labels,assignees,commentsCount,createdAt,updatedAt,author,authorAssociation,body -- -linked:pr -org:codepath -label:claimed

# "I write Python for coursework and small projects, including ... docs." and "one bounded Python bug or documentation fix"
gh search issues --label "good first issue" --label documentation --language python --state open --no-assignee --comments "<5" --created "<2026-09-23" --sort updated --limit 100 --json title,url,repository,labels,assignees,commentsCount,createdAt,updatedAt,author,authorAssociation,body -- -linked:pr -org:codepath -label:claimed

# "including FastAPI-style backends" (a web/API backend project)
gh search issues --label "good first issue" --language python --state open --no-assignee --comments "<5" --created "<2026-09-23" --sort updated --limit 100 --json title,url,repository,labels,assignees,commentsCount,createdAt,updatedAt,author,authorAssociation,body -- fastapi -linked:pr -org:codepath -label:claimed

## Ranking filters

<!-- Conditions and orderings over the free result fields, applied in
the order written. State each so a stranger could apply it and get
your ordering. Example SHAPES (write your own):

- drop any result with an assignee
- drop any result not updated in the last 60 days
- prefer fewer comments over more
-->

Apply these in order to the combined results of all three queries,
using only the fields gh returned. "Today" is the scan date.

1. Combine the three result lists and drop duplicates by `url`.
2. Drop any result with one or more `assignees`. (Profile: "issues
   someone has already claimed.")
3. Drop any result whose `repository.nameWithOwner` owner is
   `codepath`. That's the course sandbox, not the wild.
4. Drop any result whose `authorAssociation` is `OWNER`. Only a
   user-owned repo shows OWNER, and an owner who files their own
   good first issues is the brand-new personal or practice repo
   pattern from the activity. Issues in organization repos show
   MEMBER, COLLABORATOR, CONTRIBUTOR, or NONE and are kept. (Profile:
   "brand-new personal or practice repos whose owner files all the
   issues.")
5. Drop any result that has a label containing, case-insensitively,
   any of: `claimed`, `assigned`, `in progress`, `wip`, `stale`,
   `enhancement`, `feature`, `question`, `discussion`, `frontend`,
   `typescript`, `javascript`, `translation`, `i18n`, `tracking`,
   `epic`, `umbrella`, `meta`, or exactly one of: `ui`, `web`, `ci`,
   `devops`. (Profile: bug or docs, not a feature; no React/frontend,
   no CI/DevOps, no sweeps.)
6. Drop any result with `commentsCount` of 5 or more. Long threads
   mean a crowded issue or an unsettled one.
6a. Drop any result whose `body` is shorter than 80 characters once
   whitespace is trimmed. With an empty body there's no bounded change
   to grade (scan 1: open-nvr#29).
6b. Drop any result whose `body` has 6 or more task-list boxes
   (`- [ ]` or `- [x]`). That's an umbrella checklist meant to be
   split; a short acceptance-criteria list stays under 6 (scan 1:
   aeon-neuro#78 had 14).
7. Drop any result whose `createdAt` is less than 21 days before
   today, or whose `updatedAt` is more than 120 days before today.
8. Order the survivors by **fewest comments first**. Break ties by
   **most recent `updatedAt` first**.
9. Walk that order and keep only the first result from each
   repository, so the five graded issues come from five different
   repos.
10. Take the top 5.

## How many to grade

Grade the top **5** ranked results per scan.

<!-- Five is the default because grading is the only stage that costs
money: about $0.20 per candidate, so a scan is about $1, and you can
iterate the strategy several times for less than one full regression
run. You need one good issue, not ten. Raising this number is
allowed, but it is a decision you state here in a sentence, with the
cost you are accepting; it is not a number you bump because a scan
disappointed you once. -->
