# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/57

**Verdict output**

Accepted

1. #57 – Tech detector counts node_modules/ and build/ files (agent/tools/tech_detector.py). It's Python and backend only, with a copy-paste reproduction script and two named failing tests. That makes it clearly scoped and reproducible, which is what your fit profile asks for, and it needs no frontend work. There's one claim comment from ApoorvThite (Sep 21) with no PR yet, and BishalChhetri has already posted a reproduction report. Under the house rule, classmates' claim comments don't block an issue.

Rejected

- #72 – verify_password raises UnknownHashError: the maintainer, repo, scope and AI-policy checks all passed. It fails "Already claimed" because PR #78 is open, says "Fixes #72", and was opened Sep 27.
- #69 – Output parser crashes on a top-level JSON array: the maintainer, repo, scope and AI-policy checks all passed. It fails "Already claimed" because PR #79 is open, says "Fixes #69", and was opened Sep 27.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/57",
    "checks": [
      {
        "name": "Maintainer activity",
        "grade": "pass",
        "evidence": "Latest default-branch commit 2026-09-16 by Aburke225, 12 days before today (2026-09-28), within 90 days"
      },
      {
        "name": "Repository in use",
        "grade": "pass",
        "evidence": "archived: false; no releases; last push 2026-09-16, within 180 days"
      },
      {
        "name": "Newcomer-sized scope",
        "grade": "pass",
        "evidence": "Single bounded bug in tech_detector.py with repro script and two named failing tests; opened 2026-09-10, no closed/abandoned PRs"
      },
      {
        "name": "Already claimed",
        "grade": "pass",
        "evidence": "No assignees, no linked PRs; only claim comments are from students (ApoorvThite 09-21, BishalChhetri 09-28), which the Path Review house rule says to ignore"
      },
      {
        "name": "AI contribution policy",
        "grade": "pass",
        "evidence": "docs/CONTRIBUTING.md and PR template contain no AI policy; silence passes"
      }
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72",
    "checks": [
      {
        "name": "Maintainer activity",
        "grade": "pass",
        "evidence": "Latest default-branch commit 2026-09-16, within 90 days"
      },
      {
        "name": "Repository in use",
        "grade": "pass",
        "evidence": "archived: false; last push 2026-09-16, within 180 days"
      },
      {
        "name": "Newcomer-sized scope",
        "grade": "pass",
        "evidence": "One function in core/security.py plus xfail marker removal; maintainer estimate 1-2 hours"
      },
      {
        "name": "Already claimed",
        "grade": "fail",
        "evidence": "Open linked PR #78 'fix: verify_password fails closed on malformed stored hashes' (Fixes #72), opened 2026-09-27"
      },
      {
        "name": "AI contribution policy",
        "grade": "pass",
        "evidence": "No AI policy in docs/CONTRIBUTING.md or PR template; silence passes"
      }
    ],
    "verdict": "reject"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/69",
    "checks": [
      {
        "name": "Maintainer activity",
        "grade": "pass",
        "evidence": "Latest default-branch commit 2026-09-16, within 90 days"
      },
      {
        "name": "Repository in use",
        "grade": "pass",
        "evidence": "archived: false; last push 2026-09-16, within 180 days"
      },
      {
        "name": "Newcomer-sized scope",
        "grade": "pass",
        "evidence": "Single fallback-path bug in rag/generator/output_parser.py; maintainer estimate 2-4 hours"
      },
      {
        "name": "Already claimed",
        "grade": "fail",
        "evidence": "Open linked PR #79 'fix: handle top-level JSON arrays in output parser' (Fixes #69), opened 2026-09-27"
      },
      {
        "name": "AI contribution policy",
        "grade": "pass",
        "evidence": "No AI policy in docs/CONTRIBUTING.md or PR template; silence passes"
      }
    ],
    "verdict": "reject"
  }
]
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

16/20 → 1/4 targeted → 1/1 issue-01 → 1/1 issue-19 → 1/1 issue-15 → 3/3 targeted → 19/20 full run → 18/20 final saved run.

**Issue analysis**

`issue-15` — My rubric decided `reject`, and the gold label was also `reject`. The final eval recorded: `"issue-15  reject  reject   yes"`.

The issue had been open for several years and had multiple abandoned contributor attempts and closed unmerged PRs. My revised `Newcomer-sized scope` check treats that history as evidence that the issue may be more difficult than it first appears, so the rubric rejected it.

**Check rationale**

Current check:

`Newcomer-sized scope | Issue body, comment thread, issue open date, and linked PR history | Pass unless the issue is explicitly an umbrella or tracking issue, is purely a usage/support question, has unresolved design debate with no maintainer decision, or a maintainer states that the fix requires changes to core internals. Also fail if the issue has been open for more than 1 year and has at least 2 closed unmerged PRs or at least 3 clearly abandoned contributor attempts. Multiple related files, checklist items, or detailed acceptance criteria do not fail this check by themselves. | required`

I chose this wording because my first version was too strict and rejected issues just because they involved several related files or had a long description. I revised it to focus on concrete warning signs instead: umbrella scope, support-only questions, unresolved design work, core-internal changes, or a long history of failed contribution attempts.

**Trade-offs**

This check can still reject an issue that is technically solvable but has a long history of abandoned attempts. I accepted that trade-off because the goal is to find a good first contribution, not just any open issue.

I tested the revised rule with targeted runs on `issue-01`, `issue-15`, and `issue-19`. After the change, all three matched their gold labels in the targeted run: `3/3 scored items`.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. This issue fits my interests because it is a Python tooling bug involving file detection logic, which is close to the kind of backend, scripting, and automation work I am comfortable with. It is also small enough to complete within the time available because the issue includes a clear reproduction case and named failing tests.

2. The verdict correctly identified that the issue is active, unassigned, reproducible, and appropriately scoped for a first contribution. The rubric could not fully measure my personal comfort with Python tooling and path-filtering logic, which also made this issue a better fit for me than the other accepted candidates.

3. I expect the claim process to be straightforward. The issue is still open and has no assignee or linked PR. There are student claim comments, but the Path Review house rule says those do not block the issue.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
