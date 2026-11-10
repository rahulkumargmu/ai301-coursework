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
**Pull Request:** https://github.com/TiBillet/Lespass/pull/495

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

**Unit 7: Wrote a ship manifest for TiBillet/Lespass, installed the ship-gate, and got a BLOCK on a 339-file diff that turned out to be my stale local main**

My `ship-manifest.md` has three must-pass rows taken from `GUIDELINES.md` and `tests/README.md` (`manage.py check`, ruff on my new test file, and the three membership test files) and four excluded rows dated 2026-11-09: ruff on all of `models.py` shows the same 9 findings on main, `make test` stops on macOS bash 3.2, 4 Stripe tests fail on main with "You did not provide an API key", and `make e2e` needs a Stripe test account. Before the install, pr-precheck refused to grade the branch because its scope was still the Path Review sandbox; `install.py` swapped in the wild scope on 2026-11-10, after the PR was already open. The first `check_gate.py --branch` run passed all three repo checks, then stopped: "plan-fidelity [fail]: Diff stat is 339 files … Administration/admin_tenant.py (+15)" and "diff-reviewable [fail]: … a commented-out block in Administration/admin/dashboard.py". My branch touches 4 files; the gate diffs against the local `main`, which was still my fork's 2026-10-21 main, 59 commits behind `upstream/main`. I fast-forwarded local `main` to `upstream/main` (`git branch -f main upstream/main`), and the next run said `ship-gate: PASS` with all four pr-precheck checks passing. I changed nothing in the rubric: it was right to reject a diff it could not trace to the plan.

**Unit 8: Installed the PR monitor, wrote my playbook from five merged Lespass PRs, and kept its first draft off the thread**

The playbook's staleness numbers come from the repo, not a guess: Lespass has no CONTRIBUTING.md, and the last five merged PRs from outside the core team (#463, #468, #471, #472, #488) got their first maintainer action in 0 to 4 days, so I nudge after 7 quiet days and call a review stalled after 14; the nudge rules start from my voice guide's pings rule and tag only NothRen. On 2026-11-10, `monitor.py --state` reported "FEEDBACK-IN: review feedback is in", but the only review on PR #495 was Copilot's automatic notice that it could not review because of a quota limit. The full run drafted a wait note instead of a reply ("Don't post anything on PR #495 today … Check again on 2026-11-17"), so I posted nothing. Because the state line had counted a bot notice as feedback, and the draft said my playbook did not cover re-requesting a Copilot review, I added a Response rule: a bot's notice is not a review round, gets no reply, and I don't re-request Copilot on someone else's repo.

---

## Why I Chose This Issue

Last week's scan accepted two issues. On 2026-10-21 I graded them again after replacing `scope-fits` with `scope-feasible`. workfloworchestrator/orchestrator-core#1872 came back reject: nightcityblade had already claimed it, and merged PR #2021 closed it. TiBillet/Lespass#450 came back accept, difficulty moderate. It is one Python/Django bug: cancelling a membership from the admin leaves `deadline` set, so the date on screen can still be in the future. Nobody is assigned and there are no other comments. I want a small bug I can finish in two weeks, in a project where maintainers are actually pushing (this repo was pushed the same day). The issue text is in French, which I can read with help, and the stack is heavier than a one-file fix. I hope to learn how this project separates "access is cut" (`is_valid()` is false for `ADMIN_CANCELED`) from "the date on the card".

The scout was right that #1872 was no longer free, and right that #450 is one piece of work. It was wrong last week to call both repos silent on contributing docs. It also cannot see that the issue author already pushed `[Fix issue #450]` commits on 2026-09-30; those commits are on main and are not a claim comment, so `unclaimed` still passed. I have not run the admin page yet.

Correction, 2026-11-09: after I fetched the full history of my fork, `git log -S "membership.deadline = datetime.now()"` showed that the author reverted that fix in `c2ca56f` ("Revert commit d0ea1ff8", 2026-10-03) and removed the `fix-in-V2` label on 2026-10-12. My clone was shallow (one commit), so I could not see this on 2026-10-21. Neither could the scout: it reads the issue page, not the commit history. The revert is why my fix changes the display and leaves `deadline` alone.

Claim comment: https://github.com/TiBillet/Lespass/issues/450#issuecomment-6330574158

**Toolkit**

- Tool: issue-scout. Its $1.02 scan on 2026-10-14 ran my three `gh search issues` queries (293 results, 69 after my ranking rules) and graded the top five with my rubric; #450 was one of its two accepts. On 2026-10-21, after the scope patch, I ran it in direct grade mode on both accepts to choose between them.
- What it produced: Lespass#450 accept, difficulty moderate. orchestrator-core#1872 reject on `unclaimed`.
- Difficulties: `unclaimed` only reads assignees, comments, and linked PRs. It did not treat the author's September fix commits as a claim, and it cannot see that the fix was reverted, which mattered more for my plan than for my claim.
- What changed: On 2026-10-21 I replaced `scope-fits` with `scope-feasible`. The old check rejected any open edge. The new check fails only when there is no single piece of work, and an open edge becomes a difficulty note instead of a reject. Next I would have the scout run `gh api .../issues/<n>/timeline` and report "referenced by commit" events, so a fix-then-revert shows up before I claim.

---

## Understanding the Issue

**Problem Description**

When a venue manager cancels someone's membership from the Lespass admin, the member's card on "Mon compte" says "A expiré dans 11 mois" ("expired in 11 months"). The sentence contradicts itself: something that already expired cannot expire in the future. The member cannot tell from the card whether the membership still counts, and the "Renouveler" button under it makes it look like an ordinary expired membership.

**Expected Behavior**

After an admin cancel, the card should say that the membership was cancelled and when, for example "Annulée le 9 novembre 2026", with no future expiry date. Access should stay cut the way it is now (`is_valid()` returns `False`), and the paid `deadline` can stay as it is, because the project reverted the change that rewrote it.

**Current Behavior**

`MembershipMVT.cancel` sets `status = ADMIN_CANCELED` and `annulee_le = now`, but keeps `deadline`, the end of the period the member paid for (a year away in my reproduction). `Membership.is_valid()` returns `False` for `ADMIN_CANCELED`, so the account view marks the card as not valid, and the card's not-valid branch prints `{% translate 'Expired' %} {{ membership.deadline|naturaltime }}`. With the deadline in the future, `naturaltime` gives "dans 11 mois, 3 semaines", so both skins (`classic` and `V2`) show "A expiré dans 11 mois, 3 semaines".

**Affected Components**

- `BaseBillet/views.py`, `MembershipMVT.cancel`: sets the status and `annulee_le`, leaves `deadline` (not changed by my fix).
- `BaseBillet/models.py`, `Membership.is_valid()` and the template helper properties next to it (`est_resiliee_en_cours`).
- `BaseBillet/views.py`, `MyAccount.membership`: computes `membership.est_valide` inside each tenant before rendering.
- `pages/templates/pages/classic/vues/compte/membership/membership_card.html` and `pages/templates/pages/V2/vues/compte/membership/membership_card.html`: the footer line that prints the expiry.
- `locale/fr` and `locale/en`: the strings "Expired", "Annulée le", and "Annulé par un admin" already exist.

**Toolkit**

- Tool: my repo memory file, `CLAUDE.local.md` in my Lespass clone (Unit 6), which already named `MembershipMVT.cancel`, `is_valid()`, and the two cards; plus `git log -S` on the full history to see what the maintainer had tried.
- What it produced: the component list above, and the fix-then-revert history (`d0ea1ff`, `ba54a51`, `c2ca56f`) that I would otherwise have repeated.
- Difficulties: the memory file said "This machine does not have Docker", which was wrong (the CLI was installed and only the daemon was stopped), and my clone was shallow, so `git log` showed one commit and no revert.
- What changed: on 2026-11-09 I ran `git fetch --unshallow`, added `upstream`, and rewrote the memory file's Build and test section with the lab setup that works on this Mac, the inactive-admin trap after a flush, and the `make test` problem.

---

## Reproduction Process

**Environment Setup**

- macOS 26.5.2 (build 25F84), Apple Silicon (arm64). Docker Desktop 27.3.1, Docker Compose v2.29.7.
- Lespass `main` at `6e44a0d` (2026-11-07, `VERSION=2.1.19`), on branch `fix/450-cancelled-membership-card` before any change. The issue was filed on 2026-07-15 and names no version.
- Inside `lespass_django`: Python 3.11.18, Django 4.2.30. `lespass_postgres`: PostgreSQL 13.23.
- Setup, from `README.en.md`'s quick start:

```bash
git fetch --unshallow origin
git remote add upstream https://github.com/TiBillet/Lespass.git && git fetch upstream
git switch -c fix/450-cancelled-membership-card upstream/main
cp env_example .env
mkdir -p certs && openssl req -x509 -newkey rsa:2048 -nodes -days 30 \
  -keyout certs/dev-key.pem -out certs/dev.pem -subj "/CN=tibillet.localhost" \
  -addext "subjectAltName=DNS:tibillet.localhost,DNS:*.tibillet.localhost"
docker network create frontend
docker compose up -d --build
docker compose stop lespass_celery
docker exec lespass_django ./flush.sh --no-runserver
docker compose start lespass_celery
```

- Two local-only fixes, in a `docker-compose.override.yml` kept out of git: `fedow_django` needs `platform: linux/amd64` (the `tibillet/fedow` image has no arm64 build: "no matching manifest for linux/arm64/v8"), and the four `*.tibillet.localhost` hosts point at `host-gateway`, because `172.17.0.1` does not reach Traefik under Docker Desktop. I used a self-signed certificate instead of mkcert.

First attempt, 2026-10-21: the Docker daemon was not running, so I only printed the cancel code and never opened the admin. Repro-check rejected that report and I did not post it.

**Steps to Reproduce**

1. Run `docker exec -i lespass_django poetry run python manage.py shell < repro_450.py`. The script (posted in full on the issue) does steps 2 to 5 and deletes what it created at the end.
2. In tenant `lespass`, create a member and a paid yearly membership whose `deadline` is 360 days away, and print its status, `deadline`, and `is_valid()`. The membership is created directly, not through the membership form, because that form goes through Stripe and I have no Stripe test keys.
3. As the venue admin (`admin@admin.com`, set active first, because `flush.sh` leaves it inactive), `POST /memberships/<pk>/cancel/` with `with_credit_note=0`. This is the request the admin's Cancel button sends.
4. Print the membership again: status, `annulee_le`, `deadline`, `is_valid()`.
5. Log in as the member, `GET /my_account/membership/`, and print every "A expiré…" or "Annulée le…" phrase on the page. Then render both card templates for the same membership.

**Reproduction Evidence**

- **Commit showing reproduction:** none. The script is in the reproduction comment: https://github.com/TiBillet/Lespass/issues/450#issuecomment-6760867051
- **Screenshots/logs:** output on `main` at `6e44a0d`, 2026-11-09 (Django log lines trimmed):

```
before cancel  status: A | deadline: 2027-11-04 | is_valid: True
admin POST /memberships/<pk>/cancel/ -> 204
after cancel   status: AC | annulee_le: 2026-11-09 | deadline: 2027-11-04 | is_valid: False
GET /my_account/membership/ -> 200
  member page says: A expiré
  member page says: A expiré dans 11 mois, 3 semaines
  classic card says: A expiré dans 11 mois, 3 semaines
  V2 card says: A expiré
  V2 card says: A expiré dans 11 mois, 3 semaines
```

- **My findings:** reproduced. The admin cancel cuts access (`is_valid: False`) and dates the cancel (`annulee_le: 2026-11-09`), but `deadline` stays 2027-11-04, so the member's page says "A expiré dans 11 mois, 3 semaines", the same text as the screenshot in the issue ("A expiré : dans 11 mois, 4 semaines"). Both skins do it. I posted this report on the issue on 2026-11-10, with one question about the reverted fix. The Unit 6 tab said to keep the report off the thread and let the PR carry it; I posted it anyway because the claim had promised this output on the thread.

**Toolkit**

- Tool: repro-check (live mode, wild scope), grading my posted claim together with `repro-comment.md`.
- What it produced: on 2026-10-21, reject (`steps-followable` and `behavior-shown` failed: I had printed source lines instead of cancelling a membership). On 2026-11-09, accept with all seven checks passing; `behavior-shown` quoted "classic card says: A expiré dans 11 mois, 3 semaines".
- Difficulties: `env-recorded` would only pass with a version, and my first draft named a commit but not the project version, and did not say that the membership was created directly instead of bought. The grader also pointed out a tension my rubric cannot score: my claim said "I won't change any code until that output is up", and the fix branch existed before I posted the output.
- What changed: I added `VERSION=2.1.19` and a sentence saying how my steps differ from the issue's before grading again. For the tension, the report now says the change is "on a branch with tests", so a maintainer sees the order of events.

---

## Solution Approach

**Analysis**

The data is what the project wants and the display is wrong. `MembershipMVT.cancel` (`BaseBillet/views.py`) sets `ADMIN_CANCELED` and `annulee_le`, keeps `deadline`, and answers `204`. `Membership.is_valid()` (`BaseBillet/models.py`) returns `False` for `ADMIN_CANCELED`, so `MyAccount.membership` sets `membership.est_valide = False`. In both `membership_card.html` templates, the not-valid footer then prints `{% translate 'Expired' %} {{ membership.deadline|naturaltime }}`. Nothing on that path looks at the status, so a cancelled membership with a future `deadline` reads "A expiré dans 11 mois".

The maintainer already tried the other fix: `d0ea1ff` and `ba54a51` set `membership.deadline = datetime.now()` in `cancel()`, and `c2ca56f` reverted it on 2026-10-03. The current code dates the cancellation in `annulee_le` instead ("La date d'annulation sert au récap du matin").

**Proposed Solution**

Change only what the card shows for an admin-cancelled membership. A new plain-field property, `Membership.est_annulee_par_un_admin`, returns `self.status == Membership.ADMIN_CANCELED`, like `est_resiliee_en_cours` (no query and no save, so it is safe in templates; `tests/pytest/test_membership_gabarits_attributs.py` explains why that matters). In both card templates, the not-valid footer shows "Annulée le <annulee_le|date>" for those memberships, or "Annulé par un admin" when `annulee_le` is empty (memberships cancelled before that field existed). Both strings are already translated in `locale/`.

Not changing: `cancel()` and `deadline` (the reverted approach), `is_valid()`, the member-cancelled `CANCELED` display, the "Période du … au …" line, the V2 badge (it still says "A expiré"; I asked the maintainer about it in the PR), and the admin's user card in `Administration/admin_tenant.py`.

**Implementation Plan**

1. `BaseBillet/models.py`: add `est_annulee_par_un_admin` under `est_resiliee_en_cours`, with a bilingual docstring.
2. `pages/templates/pages/classic/vues/compte/membership/membership_card.html`: in the `{% if not membership.est_valide %}` footer, branch on `est_annulee_par_un_admin`.
3. `pages/templates/pages/V2/vues/compte/membership/membership_card.html`: the same branch in the not-valid footer.
4. `tests/pytest/test_membership_annulee_par_un_admin.py`: both skins with and without `annulee_le`, a lapsed-membership control, and the admin-cancel-then-account-page path.
5. Re-run `repro_450.py` and the manifest checks, then compare the full backend suite on `main` and on the branch.

Deviations, recorded 2026-11-09: none in the code; the plan held. `make test ARGS=...` does not run on this Mac, so I ran the same `docker exec ... pytest` it ends in. The admin cancel answers `204`, not a redirect, so the path test asserts `204`.

**Toolkit**

- Tool: plan-check (live mode, wild scope), on `plan.md` (this section plus file paths and a test plan) and the approach paragraph of my reproduction comment as the plan comment (also saved as `plan-comment.md`).
- What it produced: accept. All five required checks passed; `cause-grounded` matched the plan's cause to "deadline 2027-11-04 unchanged, is_valid False, and 'A expiré dans 11 mois, 3 semaines'", and `thread-convention` passed because the comment names `c2ca56f` and says it deliberately keeps `deadline`.
- Difficulties: my `procedure.md` reads "Thread highlights" and a "repo-facts policy line", which are eval-bundle words, so in live mode the grader had to map them to the issue comments and `GUIDELINES.md` and said so. My `thread-convention` check only reads comments, but here the maintainer's direction was a commit and its revert, not a comment.
- What changed: the plan comment names the reverted commit itself instead of relying on the check to find it. Next I would add one procedure line: in live mode, read the issue's "referenced by commit" events as maintainer direction.

---

## Testing Strategy

Two observable checks prove the fix.

1. The reproduction, re-run on the branch. Before (`main`): "classic card says: A expiré dans 11 mois, 3 semaines" and "V2 card says: A expiré dans 11 mois, 3 semaines". After (branch), same script, same commands:

```
after cancel   status: AC | annulee_le: 2026-11-09 | deadline: 2027-11-04 | is_valid: False
GET /my_account/membership/ -> 200
  member page says: A expiré
  member page says: Annulée le 9 novembre 2026
  classic card says: Annulée le 9 novembre 2026
  V2 card says: A expiré
  V2 card says: Annulée le 9 novembre 2026
```

`deadline` and `is_valid()` are the same as before; only the footer text changed. The "A expiré" left is the V2 badge, which is out of scope and asked about in the PR.

2. The repo's own checks, run inside `lespass_django`:
   - New tests without the fix (only the test file present): `5 failed, 2 passed` (the 2 passes are the lapsed-membership control). With the fix: `7 passed`.
   - `manage.py check`: "System check identified no issues (0 silenced)."
   - `ruff check tests/pytest/test_membership_annulee_par_un_admin.py`: "All checks passed!"
   - `pytest tests/pytest/test_membership_annulee_par_un_admin.py tests/pytest/test_membership_resiliation_fin_de_periode.py tests/pytest/test_membership_gabarits_attributs.py`: `15 passed`.
   - Full `pytest tests/pytest/ booking/tests/`: `371 failed, 4406 passed` on `main` and `371 failed, 4413 passed` on the branch. The failing set is the same 375 test ids on both (`comm` of the two sorted lists is empty), and the failures I opened end in `stripe._error.AuthenticationError: You did not provide an API key`.

**Toolkit**

- Tool: my `ship-manifest.md` for TiBillet/Lespass (Unit 7), run by hand, row by row, because the ship-gate was not installed yet.
- What it produced: the three must-pass rows exit 0 on the branch (output above), and four excluded rows dated 2026-11-09 with a reason each.
- Difficulties: the project's own runner, `make test`, stops on macOS bash 3.2 with `options_du_terminal[@]: unbound variable` when it has no terminal, and the full suite has 371 failures that come from missing Stripe keys, so "the suite passes" was never going to be true here.
- What changed: I wrote the must-pass rows as the `docker exec ... pytest` command `scripts/lancer_tests.sh` ends in, moved `make test` and the Stripe tests to the excluded table, and compared the full suite's failing ids on `main` and on the branch instead of trusting a count.

---

## Implementation Notes

**2026-10-21**

First reproduction attempt. `docker ps` said "Cannot connect to the Docker daemon". I printed the cancel code, `is_valid()`, and the admin card instead. Repro-check rejected the report and I kept it off the thread.

**2026-11-09**

Fetched the full history (`git fetch --unshallow`) and found the maintainer's fix and its revert with `git log -S "membership.deadline = datetime.now()"`: `d0ea1ff`/`ba54a51` set `deadline` to now, and `c2ca56f` reverted it on 2026-10-03. Decided to fix the display and leave `deadline` alone. Branched from `upstream/main` (`6e44a0d`), which was 59 commits past my old clone.

**2026-11-09**

Docker lab. The first `docker compose up` failed on `fedow_django` ("no matching manifest for linux/arm64/v8"); `platform: linux/amd64` in an override fixed it. Then `install` could not reach Fedow: the containers map `fedow.tibillet.localhost` to `172.17.0.1`, which does not reach Traefik on Docker Desktop. I tried `host-gateway`, but Compose merged both entries and the old one won, so I used `extra_hosts: !override`. A wrong turn: I tested from the container with `curl`, which kept failing because curl sends every `*.localhost` name to 127.0.0.1 itself; Python `requests` got `200 {"message":"Hello world!"}`. `flush.sh` then finished (6 tenants, demo data).

**2026-11-09**

First run of the reproduction script: the admin cancel answered `403`. `admin@admin.com` is inactive right after a flush (the test conftest reactivates it too). After setting it active, the script reproduced the bug: "A expiré dans 11 mois, 3 semaines" in both skins.

**2026-11-09**

Built the fix (one property, two template footers) and the test file. The admin-cancel path test failed at first because I expected `200` or `302`; the view answers `204` with HX-Refresh, so the test now asserts `204`. Checked that the new tests fail without the fix (`5 failed, 2 passed`). `make test` failed on bash 3.2, so I ran pytest with `docker exec` directly. Ran the full suite on the branch (11 min) and on `main` (6 min) and compared the failing ids: identical.

**2026-11-10**

Committed the fix (`fix(adhesion): la carte d'une adhésion annulée par un admin affiche « Annulée le … » (#450)`), pushed `fix/450-cancelled-membership-card` to my fork, posted the reproduction on #450, and opened PR #495 against `main`. Both posts went through Claude Code, and the voice guard hook passed each one.

**2026-11-10**

Ran the ship-gate install in my own terminal, then `check_gate.py --branch`. It blocked on a 339-file diff: my local `main` was my fork's old main, 59 commits behind `upstream/main`, so every upstream commit since 2026-10-21 looked like part of my branch. `git branch -f main upstream/main` brought the diff back to my 4 files, and the second run passed. I added the `locale/` grep the gate's voice note asked for to the PR description. Then ran the PR monitor: `--state` said FEEDBACK-IN because of Copilot's quota notice, the draft was a wait note, and I posted nothing.

**Toolkit**

- Tool: the voice guard. I ran `check_voice.py --draft` on `repro-comment.md` and `pr_draft.md` before posting, and the hook checked the `gh issue comment` and `gh pr create` commands when they ran.
- What it produced: five passes in `~/.claude/voice-guard/guard-log.md`: three `[manual]` (the reproduction comment before and after I added the version line, and the PR description) and two `[hook]` (the comment at 2026-11-10 01:38 and the PR at 01:39).
- Difficulties: the guard only checks my voice rules. It passed the reproduction comment even though the Unit 6 tab said to keep the report off the thread, because none of my rules says where a report belongs.
- What changed: nothing in the guard yet. I would add an Upstream rule for reports: post a reproduction on the thread only when the claim promised it there, and say so in the comment.

---

## Pull Request

**PR Link:** https://github.com/TiBillet/Lespass/pull/495

**PR Description:** Opened 2026-11-10 against `TiBillet/Lespass` `main` from `rahulkumargmu:fix/450-cancelled-membership-card`, titled "Carte d'adhésion : afficher « Annulée le … » après une annulation admin (#450)". It starts with `Fixes #450`, and GitHub links it to the issue. It explains the bug in French and English, lists the four changed files and what each change does, says what I left alone and why (`cancel()` and `deadline`, citing `c2ca56f`), pastes the before and after lines of the reproduction, the new tests failing on `main` (5 failed, 2 passed) and passing on the branch (7 passed), `manage.py check`, ruff on the new file, the 15 membership tests, and the full-suite comparison (371 failed on both, same ids, Stripe keys). It also notes the `make test` problem on macOS, asks one question (should the V2 badge change too), and says what Claude helped with: setting up the Docker lab, writing the reproduction script and the tests, and drafting the description.

**Maintainer Feedback:**

- 2026-11-10: none yet from a maintainer. The only review is Copilot's automatic notice that it "was unable to review this pull request" (quota). It asks nothing, so I sent no reply; the PR monitor's draft agreed and set 2026-11-17 as the first day a nudge is allowed.

**Status:** Awaiting review

**Toolkit**

- Tool: the ship-gate (Unit 7), run by hand with `check_gate.py --branch --title "…" --body-file pr_draft.md`: my manifest's three checks, then my pr-precheck on the branch, `plan.md`, `test_evidence.md`, and `pr_draft.md`. The voice guard hook checked `gh pr create`.
- What it produced: first run, BLOCK (2026-11-10 03:26): repo checks ok, but `plan-fidelity` and `diff-reviewable` failed on "Diff stat is 339 files". Second run, PASS (03:30): "All 4 diff paths … are in the plan's Files list", and the voice notes flagged one unbacked claim, "Les deux libellés existent déjà dans `locale/`".
- Difficulties: the PR opened before the gate could run, because `install.py` (the step that adds the hook and swaps pr-precheck's sandbox scope for the wild one) was blocked in my Claude Code session, so I ran it in my own terminal afterwards. The BLOCK was not about my code: the gate diffs against the local `main`, and mine was my fork's 2026-10-21 main, 59 commits behind `upstream/main`.
- What changed: I fast-forwarded local `main` to `upstream/main` before re-running, and added the `grep` of both `msgid` lines from `locale/en/LC_MESSAGES/django.po` to the PR description (`gh pr edit 495`), so the translation claim is backed. Next time I run the gate before `gh pr create`, the order Unit 7 gives.

---

## Learnings and Reflections

**Technical Skills Gained**

- Running a multi-service Django dev stack (django-tenants, Postgres, Redis, Memcached, Celery, Traefik, Fedow) under Docker on Apple Silicon: `platform: linux/amd64` for an amd64-only image, `host-gateway` for container-to-host traffic, and Compose's `!override` tag when a merged list keeps the old entry.
- Reading a project's history with `git log -S` to find a change and its revert, and why a shallow clone hides that.
- How Lespass keeps templates safe across tenants: views compute `est_valide` inside `tenant_context`, and templates only read attributes or plain-field properties, never methods that query or save.
- Writing a regression test that renders both skins of one template, plus an end-to-end test that drives the admin cancel with Django's test client and reads the member's page.
- Proving a change did not break anything when the suite cannot be green on my machine: same failing ids on `main` and on the branch.

**Challenges Overcome**

- Docker: the daemon was off on 2026-10-21, and on 2026-11-09 the stack failed twice (no arm64 Fedow image, then `172.17.0.1` unreachable). Each failure is in the 2026-11-09 Implementation Notes with its fix.
- A misleading test: `curl` from inside the container said Fedow was down because curl resolves `*.localhost` to 127.0.0.1 by itself. Python `requests`, which reads `/etc/hosts` like Django does, got a `200`.
- The reverted fix: the obvious change was the one the maintainer had already backed out. Finding `c2ca56f` turned my plan from "reset `deadline`" into "change the card".
- A test runner that does not run here: `make test` fails on macOS bash 3.2, so I ran the command it wraps and recorded why in the manifest.

**What I'd Do Differently Next Time**

- Clone with full history from the start and read every commit that references the issue before claiming it. My 2026-10-21 notes said the author's fix commits were "on main"; they were, and so was their revert.
- Start the Docker lab in the first week. My reproduction waited 19 days on a stopped daemon, and the claim's promise to post output sat unanswered that whole time.
- Follow the order the course gives (ship-gate pass, then PR) instead of opening the PR first and running the gate afterwards.
- Ask the maintainer the open question (display fix or `deadline` fix) before building, not in the same hour as the PR.

---

## Resources Used

- Lespass `README.en.md`, "Quick start": the `.env`, certificate, `docker network create frontend`, and `flush.sh --no-runserver` sequence I followed. https://github.com/TiBillet/Lespass/blob/main/README.en.md
- Lespass `GUIDELINES.md`: tests and ruff only inside the container, and the warning about `ruff --fix` and `ruff format` on existing files. https://github.com/TiBillet/Lespass/blob/main/GUIDELINES.md
- Lespass `tests/README.md` and `tests/pytest/conftest.py`: the suite runs on the dev database, the `admin_client` and `tenant` fixtures, and the admin reactivation after a flush.
- Lespass `tests/pytest/test_membership_gabarits_attributs.py` and `test_membership_resiliation_fin_de_periode.py`: the patterns my test file copies (fixtures that clean up, patching `FedowAPI`, rendering the card).
- The maintainer's commits on #450: `d0ea1ff` and its revert `c2ca56f`, which decided my approach.
- Docker Compose docs on merging files and the `!override` / `!reset` tags: https://docs.docker.com/compose/how-tos/multiple-compose-files/merge/
- Django docs for `naturaltime` (django.contrib.humanize), which explained why a future date reads "dans 11 mois": https://docs.djangoproject.com/en/4.2/ref/contrib/humanize/#naturaltime
- Claude Code (Claude, an AI assistant) helped set up the Docker lab, write the reproduction script and tests, run the checks, and draft this story; the commands ran on my machine and the outputs are copied from those runs.
