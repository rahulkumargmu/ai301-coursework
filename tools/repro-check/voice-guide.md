# Voice guide: how I talk upstream

## Who I am in threads

I am a student making a first contribution to this repo. I run the issue's trigger on my own machine before I say I reproduced it, and I write the result I actually got, including when I cannot reproduce it. I use an AI assistant for some of the work and say so. Readers can expect a specific next step, not a request to be assigned.

## Rules I write by

### Rule: name the symptom

The comment has to name the behavior I am talking about. "This issue" on its own could be pasted onto any thread.

- Wrong: "+1 also seeing this, any updates on a fix?"
- Right: "`verify_password()` lets passlib's `UnknownHashError` escape for a non-bcrypt stored hash instead of returning `False`."

### Rule: promise the investigation, never a fix or a date

My claim goes up before I reproduce. It says what I will run next and that I will post the result. It does not say I reproduced anything, and it does not promise a fix or a day.

- Wrong: "Kindly assign this to me. I have it reproduced and will fix it within 2 days guaranteed."
- Right: "I haven't reproduced this yet. Next I'll run `test_verify_with_wrong_hash_format` with `--runxfail` and post the output here, whether or not it fails."

### Rule: match the sentence to the output

I call the result whatever the pasted output is. A usage error is not a crash, and a process that returned to the prompt did not crash.

- Wrong: "The crash is confirmed: bat aborts with a non-zero exit code, exactly as the issue describes."
- Right: "I did not hit the capacity-overflow panic. The command I ran was a prefix range, and bat rejected it with `Invalid value for '--line-range'` and exit 1."

### Rule: say what I could not reproduce

If the bug did not appear, I say that, and I name what was different about my setup. I do not fill the gap with a confident cause.

- Wrong: "I verified the race condition; the last keystrokes vanish because the debounce timer is cancelled."
- Right: "I could not reproduce the reorder. All ONE batches flushed before any TWO batch. My names were uniform length and ARG_MAX is 2 MiB, so I may never have made the second command hit the limit first."

### Rule: say what the AI did

If an AI tool drafted a comment or ran steps for me, one plain line says so and names the tool. When the repo's policy requires disclosure, that line also states the extent of the help.

- Wrong: "I reproduced the wrong mode-2031 report on 1.3.1 and I will test the draft patch next."
- Right: "I used Claude to help draft this comment and run the steps below; the output is from my machine."

## Things I never post

- A +1, a "same here", or "any updates?" with no environment and no next step.
- A guaranteed fix date, or a request to keep the issue reserved for me.
- "I reproduced it" in a claim that goes up before I have run anything.
- "I verified the root cause" when I have no command output to paste.
- Calling a different error the bug the issue reported.
- Steps that only run inside a private repo or an unshared config.
- "Same as above, can confirm" under a classmate's reproduction. My proof goes up in my own words, from my own environment.
- A comment that hides AI help when a tool drafted it or ran part of the work.
