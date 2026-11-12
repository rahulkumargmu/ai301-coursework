# I opened a Lespass pull request so a cancelled membership shows the cancellation date

## Overview

Lespass is an open source Django site that venues use for tickets and memberships. [Issue #450](https://github.com/TiBillet/Lespass/issues/450) is about a member's card that still says "A expiré dans 11 mois" ("expired in 11 months") after a manager cancels the membership. I reproduced that on current main and opened [pull request #495](https://github.com/TiBillet/Lespass/pull/495). On November 12, 2026 it is still open, and no maintainer has reviewed it. I left the paid `deadline` as it was and changed the footer text in both card skins to "Annulée le …". I also wrote `ship-manifest.md`, the command list my ship-gate runs. The first run stopped me because local `main` was behind.

## What happened

On October 21 I tried to reproduce the issue and did not get there. Docker was not running, so I printed the cancel code and never opened the admin page. My reproduction check rejected that report, and I left it off the thread.

On October 22 I claimed the issue and said I would post `membership.deadline` from a cancel on current main before I changed any code.

I came back on November 9. The clone had one commit, so I fetched the rest of the history. The maintainer had already set `deadline` to now inside `cancel()` (`d0ea1ff` and `ba54a51`) and reverted that on October 3 in `c2ca56f`. I was about to make that same change. I rewrote the plan that afternoon: leave `cancel()` and `deadline` alone, and only change the sentence on the card for an admin cancellation.

The first real cancel returned 403. The demo admin is inactive after a database flush, and I had not turned that user back on. Once I did, the script on main at `6e44a0d` showed the bug. Access was already cut, and the date the member had paid through was still a year away:

```
after cancel   status: AC | annulee_le: 2026-11-09 | deadline: 2027-11-04 | is_valid: False
classic card says: A expiré dans 11 mois, 3 semaines
V2 card says: A expiré dans 11 mois, 3 semaines
```

The footer prints "Expired" next to `deadline` and never checks the status, so a future date comes out as "dans 11 mois".

The change is one property that only reads the status:

```python
@property
def est_annulee_par_un_admin(self):
    return self.status == Membership.ADMIN_CANCELED
```

Both `membership_card.html` files use it. If `annulee_le` is set, the footer says "Annulée le" and the date. If it is empty, the footer says "Annulé par un admin". A membership that just ran out still says "Expired" and the deadline. The same script on the branch printed "Annulée le 9 novembre 2026" on both skins. `deadline` was still 2027-11-04, and `is_valid()` was still `False`. The V2 badge still said "A expiré", and I left it.

Without the property and the template change, the new test file was 5 failed, 2 passed. The two passes are a membership that lapsed ten days ago and should still say "A expiré il y a". With the change, all 7 passed. One of them posts the admin cancel and then opens the member's page. My first version expected a redirect. The view answers 204 and tells the page to refresh, so I changed the test:

```python
assert reponse.status_code == 204, reponse.status_code
assert "Annulée le" in contenu
assert "A expiré dans" not in contenu
```

On November 10 I posted the reproduction on the issue and opened the pull request from `fix/450-cancelled-membership-card`. I ran the ship-gate after that. Its three commands, from `GUIDELINES.md` and `tests/README.md`, are in `ship-manifest.md`: Django's system check, ruff on the new test file, and the three membership test files, each with `docker exec` in the app container. Those passed. The diff check did not. From `~/.claude/ship-gate/gate-log.md`, November 10 at 03:26:

```
verdict: BLOCK
plan-fidelity [fail]: Diff stat is 339 files; only 4 are plan-named,
and undisclosed extras include Administration/admin_tenant.py (+15),
which the description says it did not touch.
```

The branch really is four files. Local `main` was still my fork from October 21, 59 commits behind `upstream/main`, so the gate treated every later upstream commit as part of my change. I moved local `main` to `upstream/main` and ran the gate again. At 03:30 the log says `verdict: PASS`, and the check that had failed now reads: "All 4 diff paths (models.py, classic and V2 membership_card.html, new test file) are in the plan's Files list." The second run passed, and the diff was the four files in the plan. It also said I had not shown that the two labels were already translated, so I added the `msgid` lines from `locale/en/LC_MESSAGES/django.po` to the description: "Cancelled on" and "Cancelled by an admin".

No maintainer has replied. The only review is Copilot saying the quota was used up. That note asks for nothing, so I left it.

## Limitations and what's next

The V2 badge still says "A expiré" after an admin cancellation. I left it so the pull request would stay on the footer, and I asked in the description whether it should say "Annulée" too. There is no answer yet. The new wording is in the pull request, not on the site people use. I also could not get a fully passing test suite on this machine. 371 tests fail on main and on my branch with `stripe._error.AuthenticationError: You did not provide an API key`. The failing test ids match, so the branch did not add failures, and the suite is still not green.

I am waiting until November 17 before I nudge anyone. The last five merged pull requests from people outside the core team got a first response within four days, so I gave it a week. If a maintainer wants the badge changed, I will change the badge and still leave `deadline` alone. On the next issue I will read the full history before I claim it, and I will run the ship-gate before I open the pull request.
