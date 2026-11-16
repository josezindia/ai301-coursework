# Contribution: [Issue Title]

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** [Student name]
**GitHub Username:** [your GitHub username, exactly as it appears on your profile - no @, no
profile URL. Your comments upstream are identified by this name.]
**Issue:** [GitHub issue link]
**Pull Request:** [GitHub PR link, once opened]

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

**Unit 5: Customized and tested the `issue-scout` skill for issue discovery and grading.**

I updated the skill’s scope and search strategy to reflect my Python, backend, infrastructure, automation, and AI systems interests. Some searches returned weak, already-claimed, or overly broad issues, and one query initially returned no useful results. I refined the search query and ranking approach, documented the problems in `scan-notes.md`, and reran the scout with improved results.

**Unit 6: Added communication safeguards, repository memory, and reproducibility checks for my real contribution.**

I installed and tested the voice guard so outgoing GitHub comments are checked against my communication rules before posting. I also created a local `CLAUDE.local.md` for `alltheplaces` with the repository conventions, relevant Isuzu spider patterns, and required test commands. The main issue I hit was that the initial reproduction used plain `curl`, which returned HTTP 403 even though the repository's Scrapy client could access the endpoint; I changed the reproduction to use Scrapy and confirmed the API returned the expected dealer data.

---

## Why I Chose This Issue

I used my `issue-scout` to search for newcomer-sized issues that matched my preference for Python, backend work, APIs, infrastructure, and automation rather than frontend-heavy changes. The scan surfaced `alltheplaces/alltheplaces` issue #8510, which asks for an Australian Isuzu UTE spider backed by an official dealer API. After updating the scout's scope-feasibility rule and re-grading the issue, the scout returned an `accept` verdict with a straightforward difficulty rating.

I chose it because it is a bounded Python contribution with a real external API, existing spider patterns to learn from, and a clear way to verify the result. It also gives me practice understanding an unfamiliar codebase before making a focused open-source contribution. The scout initially treated some open-ended design details too harshly, so I changed the rubric so those details affect the difficulty assessment instead of automatically rejecting an otherwise feasible issue.

Claim comment: https://github.com/alltheplaces/alltheplaces/issues/8510#issuecomment-6909757445

**Toolkit**

- Tool: `issue-scout`; I used its search strategy and rubric to find and re-grade open-source issues that matched my Python, backend, infrastructure, API, and automation interests.
- What it produced: It found issue #8510 and ultimately graded it `accept` with a straightforward difficulty rating after checking repository activity, issue availability, scope, and contribution fit.
- Difficulties: The original scope check could reject bounded issues simply because they contained an open design edge, and some searches returned weak, claimed, or overly broad issues.
- What changed: I replaced the old newcomer-scope check with a `scope-feasible` check so open design questions increase difficulty instead of automatically causing rejection, then re-ran the scout before choosing the issue.

---

## Reproduction Process

**Environment Setup**

I reproduced the issue on macOS 26.5.2 using `uv` 0.13.8, Python 3.11.5, and Scrapy 2.19.0. I cloned my fork of `alltheplaces/alltheplaces`, ran `uv sync`, and verified the project setup with `uv run scrapy`. The repository was at commit `7f884e7c743256597d197ea7dc1af88a8b257309`.

The main challenge was that a plain `curl` request to the Isuzu UTE dealer endpoint returned HTTP 403. I then tested the same endpoint through the project's Scrapy environment, which successfully received HTTP 200 and returned the dealer data.

**Steps to Reproduce**

1. Confirm that `locations/spiders/isuzu_au.py` does not exist in the repository.
2. Fetch the official dealer endpoint with `uv run scrapy fetch "https://www.isuzuute.com.au/isuzuapi/Dealer/GetAllDealers"` and save the returned JSON.
3. Inspect the `Dealers` array. It contains 165 records, including `7207` Blacklocks Isuzu UTE and `7208` Hunter Isuzu UTE. This matches the 165 locations stated in issue #8510.

**Reproduction Evidence**

- **Commit showing reproduction:** `7f884e7c743256597d197ea7dc1af88a8b257309`
- **Screenshots/logs:** Scrapy returned HTTP 200 and the dealer response contained 165 records. Three records are currently marked `IsActive: False`, so the final spider output may contain fewer than 165 active locations depending on the repository's expected filtering behavior.
- **My findings:** The official API is still available through the project's Scrapy client and contains the data needed for an Australian Isuzu UTE spider, including dealer identifiers, addresses, coordinates, contact details, opening hours, and Sales, Service, and Parts information. There is no existing `isuzu_au.py` spider, so issue #8510 remains actionable.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

[1-2 paragraphs in your own words: how you searched, what your scan
surfaced, what your scout's verdict said, why this issue fits your
profile, and what you hope to learn. If your scout's verdict on it
was reject and you chose it anyway, say so here and say why; choosing
against your own tool silently is what loses points. Close with an
honest read of the tool itself: where the scout fell short on a real
wild search and what you changed or would change, or what convinced
you it held up. **Record the link to your claim comment in this section too.**
Not claimed yet? Write `claim pending` and today's date where the link goes,
and add the link the moment your claim lands.]

**Toolkit**

- Tool: issue-scout (your scout: your search strategy, your rubric),
  and how you used it
- What it produced: [the scan and its verdict on this issue, described]
- Difficulties: [where it fell short, or "none met" if none did]
- What changed: [what you patched or would update, or "nothing"]

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

[What a stranger would need to stand this up: your operating system,
the relevant versions (language, runtime, package manager, the project
itself), and how you set the project up. Then the challenges you faced
and how you solved them, or that none came up.]

**Steps to Reproduce**

1. [Step 1]
2. [Step 2]
3. [Observed result]

**Reproduction Evidence**

- **Commit showing reproduction:** [link to commit in your fork]
- **Screenshots/logs:** [if applicable]
- **My findings:** [what you discovered during reproduction; an
  honest, evidenced cannot-reproduce that led you to re-pick or ask
  the maintainer belongs here and costs you nothing]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

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
