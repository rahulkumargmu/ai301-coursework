I opened a pull request on Lespass, an open source site that venues use for memberships. I changed the line a member sees after a manager cancels their membership, so it names the day of the cancellation instead of saying the membership expired about eleven months from now.

The pull request is still open, and no one on the project has reviewed it yet. I left the end date the member had paid for. An earlier change that rewrote that date had been taken back, so I only changed the wording on the card. The tests I added fail when the old sentence is still there and pass when the new one shows up. I was about to rewrite that date too, until I found that in the history. Next time I will check there before I start.

https://github.com/TiBillet/Lespass/pull/495

#OpenSource
