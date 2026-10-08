# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/107

**Branch**

fix/72-fail-closed

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Three runs, in order. The first two were partial `--only` runs and were not saved. The last is the full run in `eval-run.txt`.

1. `--only pkg-01,pkg-02,pkg-03,pkg-04,pkg-07,pkg-11,pkg-12,pkg-13,pkg-16,pkg-17,pkg-18,pkg-20` printed `agreement: 10/12 scored items`. The two nos were `pkg-02  clear-accept  accept  reject   NO     failed: test-decisive` and `pkg-13  clear-accept  accept  reject   NO     failed: test-decisive`.
2. After loosening `test-decisive`, `--only pkg-02,pkg-04,pkg-07,pkg-11,pkg-13,calib-04 --include-calibration` printed `agreement: 5/5 scored items`. `calib-04` stayed `reject  reject   yes`.
3. The confirming full run, the one in `eval-run.txt`, ends with this line:

```
agreement: 20/20 scored items  (bar: 18/20: PASS)
```

**Package analysis**

`pkg-01`. Gold label from `gold-labels.json`: `"verdict": "reject"`, note: "solid C-parser fix with decisive ASAN evidence, but pandas' required PR checklist is visibly ignored: no closes #xxxx, no checklist, and the stated whatsnew entry is absent from the diff". My rubric's verdict on the saved run: `pkg-01  standards-wall  reject  reject   yes`.

`standards-met` is the check that holds it. The package's repo-facts line says: "the PR template is a required checklist: closes #xxxx with the issue number, tests added and passed, all code checks passed (pre-commit), type annotations added to new arguments, and an entry in the latest doc/source/whatsnew/vX.X.X.rst when fixing a bug". The description is only this paragraph: "The C tokenizer under-reserves stream space when short rows pad missing fields, and a following wide field then writes past the reservation. This adds a capacity check to the bulk-copy path so the stream is regrown for the pending copy when padding has consumed the reservation. A regression test pins the short-rows-then-wide-field shape; test evidence below." That text has no `Closes #` and no checklist. The pass condition says: "Fail when a required issue reference, checklist, or changelog/whatsnew file is absent." The test evidence is real (`99999` before, `100000` after, plus an ASAN note), so `test-decisive` does not hold this package. The hold is the missing template asks.

**Check rationale**

Quoted from `tools/pr-precheck/rubric.md`, the `test-decisive` row, as it reads now:

```
| test-decisive | The plan's test plan read against the candidate PR's Test evidence section. The bug repro is the command the test plan says to re-run for the reported failure. A control, a suite command, or a happy-path check is not a second bug repro. | Pass when that bug repro is shown and its result is specific: printed output, an exit code, an error string, or a before/after written on the command, including after `#` or `--`. A control or a suite may be one sentence ("control unchanged", "pytest passes", "covered by the new test") once the bug repro itself is shown. Fail when the evidence never shows the bug repro and only says "tested locally", "works now", "verified on my machine", or "tests pass". Fail when the commands shown are only a control the plan says does not hit the bug. Fail when the test plan names two separate bug reproductions (two commands that each trigger the failure, such as repro 1 and repro 2) and one of those commands never appears. An honest pasted failure, with the reason, passes. Hiding it does not. | required |
```

The first version required every clause of the test plan to have its own pasted transcript, and it rejected `pkg-02` and `pkg-13`, both gold `accept`. `pkg-02` shows the failed-save repro with printed `'IMPORTANT OUTPUT\n'` before and after, and says the successful save is "covered by the new test's second half". `pkg-13` shows the `&` repro with `-- before` and `-- after` results, and says "No-ampersand control unchanged." I rejected the stricter rule because those are the bug repro plus a control, not two bug reproductions. The row now treats a control or a suite as one sentence once the bug repro is shown, and it still fails a second bug-triggering command that never appears.

**Trade-offs**

Counting a result written after `#` or `--` gives up the stricter reading from the activity, where a `#` note is never output. A command that is the right repro, with only "works now" after `#`, can pass `test-decisive`. I kept the two-bug-repro fail so that hole does not cover `calib-04`, whose test plan says "re-run repro 1" and "re-run repro 2" and whose evidence shows repro 1 only. The canary re-run was `--only pkg-02,pkg-04,pkg-07,pkg-11,pkg-13,calib-04 --include-calibration`. It printed `pkg-04    not-tested    reject  reject   yes`, `pkg-07    not-tested    reject  reject   yes`, and `calib-04  calib         reject  reject   yes`, so the loosen did not flip the narrative-only or control-only rejects.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
