---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Decide exactly one thing: whether one candidate pull request is ready to submit. Grade the PR package—its title, description, commits, diff, and test evidence—against the plan it claims to implement, the issue that plan belongs to, and the repository's stated standards. Do not grade unrelated work or more than one PR package in a run.

## Inputs and modes

In live mode, read the student's `plan.md`, including `## Deviations`; the branch diff from `git diff main...HEAD`; the draft PR title and description in `pr_draft.md`; the captured test output in `test_evidence.md`; and the issue named in the command. Read the real issue thread, the repository PR template, and `docs/CONTRIBUTING.md` for issue-side and standards evidence. A house-chain student uses the provided house plan, repro pack, and branch in place of their own artifacts.

In eval mode, the supplied package bundle is the whole world. Do not fetch, infer, or read anything outside that bundle. Grade every rubric check and apply the full verdict rule to that one package.

## The scope seam (live mode only)

In live mode, read `scope.md` before any other source. Refuse to grade if its `Repo:` line is still a placeholder, and tell the student to fill it with the section Path Review repository. Refuse work outside the scoped repository. Never guess a scope. Apply the house rules written in `scope.md` while grading. In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, read `voice-guide.md` and compare the outgoing PR title and description with those rules. Report any broken voice rule in the readable summary. A voice-guide violation changes the verdict only when a rubric check independently makes that requirement verdict-bearing. In eval mode, ignore `voice-guide.md`.

## Component reads

Read `rubric.md` for the checks and verdict rule. Use `references/evidence-guide.md` to locate each evidence family. Execute `procedure.md` exactly as written.

If the procedure is silent on a needed grading step, report that gap rather than inventing a new step. If `rubric.md` or `procedure.md` has no substantive content, refuse to grade and say so. HTML instruction comments do not count as content. Never invent checks at runtime.

## Verdict and output

The verdict is binary. `accept` means the package is ready to submit; `reject` means hold it. A readable per-check summary may appear before the machine-readable result. End every completed grading run with the following fenced JSON block, valid and last, with nothing after it.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

Grade from evidence, not confidence or polish. Name the concrete fact or quote that decides every check. Apply the rubric's written pass conditions even when intuition differs; revise the rubric later rather than changing a grade ad hoc. Follow the procedure as written and report procedure gaps instead of improvising. Judge the actual artifacts, not their verbosity or formatting. When the rubric's verdict rule does not say otherwise, treat an unverifiable or genuinely unclear required claim as failing.
