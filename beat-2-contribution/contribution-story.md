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

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

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

## Understanding the Issue

**Problem Description**

All the Places has no spider for Isuzu UTE dealers in Australia. Issue #8510 asks for one, and the brand publishes an official dealer API at `https://www.isuzuute.com.au/isuzuapi/Dealer/GetAllDealers` that already has the data, so the gap is a missing spider rather than a bug in existing code.

**Expected Behavior**

A new `isuzu_au` spider should read the dealer API and emit one location per Australian Isuzu UTE dealer, with the shared Isuzu brand attributes (`brand: Isuzu`, `brand_wikidata: Q29803`), the dealer's address, coordinates, contact details and opening hours, and the categories the issue lists: `shop=car`, `shop=car_repair` and `shop=car_parts`.

**Current Behavior**

`locations/spiders/isuzu_au.py` does not exist, so none of these dealers appear in the project's output. The API itself is live: fetched through the project's Scrapy client it returns HTTP 200 and a `Dealers` array of 165 records, three of which are marked `IsActive: False`. A plain `curl` request to the same URL returns HTTP 403.

**Affected Components**

- `locations/spiders/isuzu_au.py`: the new spider (the only file the change adds).
- `locations/spiders/isuzu_jp.py`: defines `ISUZU_SHARED_ATTRIBUTES`, which the new spider imports instead of repeating the brand fields.
- `locations/spiders/isuzu_th.py`: an existing Isuzu spider built on `JSONBlobSpider`, `DictParser` and `apply_category`.
- `templates/spiders/json_blob.tmpl`: the starting template for a JSON-backed spider.
- `tests/test_item_attributes.py`: checks the brand and Wikidata values against NSI.

**Toolkit**

- Tool: my repo-memory file `CLAUDE.local.md` for `alltheplaces`, plus the repository's `AGENTS.md`, which I used to find the files and conventions this issue touches.
- What it produced: the list of affected components above, the shared Isuzu attributes in `isuzu_jp.py`, and the test and run commands for checking the spider.
- Difficulties: `CLAUDE.local.md` points to `isuzu_th.py` as the current Isuzu pattern, but that spider pulls its JSON out of page scripts and has its own hours-parsing helper, so only its use of the shared attributes and category helpers carries over to a plain JSON API.
- What changed: I treat `isuzu_th.py` as a reference for brand attributes and categories only, and use the JSON blob template for the spider's structure.

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

**Toolkit**

- Tool: `repro-check`, which I used to grade my reproduction against the issue before posting.
- What it produced: an independent check that the live dealer endpoint returns 165 records, that plain `curl` gets HTTP 403 while the project's Scrapy client gets HTTP 200, and that `locations/spiders/isuzu_au.py` does not exist.
- Difficulties: the endpoint's response depended on the HTTP client, so a plain `curl` request made the API look unavailable when it was not.
- What changed: I reproduced with the project's own Scrapy client instead of `curl`, and recorded the 3 `IsActive: False` records so the final location count can be explained.

---

## Solution Approach

**Analysis**

There is no code defect behind the issue. The dealer data is already published by the brand's own API, and the repository has no spider that reads it. The pieces needed already exist: `JSONBlobSpider` can read the `Dealers` array straight from the API response, `DictParser` can map the standard fields, `ISUZU_SHARED_ATTRIBUTES` in `isuzu_jp.py` supplies the brand, and `apply_category` with `Categories.SHOP_CAR`, `Categories.SHOP_CAR_REPAIR` and `Categories.SHOP_CAR_PARTS` covers the three categories in the issue. During reproduction, the request worked through Scrapy but not through plain `curl`, so the spider should not need proxy or Playwright settings.

**Proposed Solution**

Add one small `isuzu_au` spider based on `JSONBlobSpider` that requests the dealer API, reads the `Dealers` array, imports `ISUZU_SHARED_ATTRIBUTES`, uses the source's dealer ID as `ref`, and applies categories from each dealer's Sales, Service and Parts information. Opening hours go through `OpeningHours` from `locations/hours.py`.

I am deliberately not changing the existing Isuzu spiders, the shared attributes, `locations/categories.py` (the three categories already exist there), the throttling, proxy or Playwright settings, or anything under `tests/`. I am also not adding a per-spider test file or custom cleanup for phones, whitespace or duplicates, because the pipelines already handle those.

I will not filter out the three `IsActive: False` records in the spider ahead of time. Inspecting the spider's output will show whether the repository's conventions already exclude them, and I will only add handling if they do not.

Each dealer becomes one location carrying every category that applies to it (car sales, car repair, car parts). I will only split a dealer into separate locations if its departments have separate physical addresses or coordinates in the data.

**Implementation Plan**

1. Create `locations/spiders/isuzu_au.py` from `templates/spiders/json_blob.tmpl`, pointing at the dealer API and the `Dealers` array, with `item_attributes = ISUZU_SHARED_ATTRIBUTES`.
2. In `post_process_item`, add only the brand-specific parts: the Sales, Service and Parts categories on a single location per dealer, and the opening hours. Inactive records are not filtered ahead of time.
3. Run `uv run scrapy runspider locations/spiders/isuzu_au.py -o /tmp/out.geojson -L INFO` and read several output records, not just the count.
4. Run `uv run pytest tests/test_item_attributes.py -q` and `uv run pre-commit run --files locations/spiders/isuzu_au.py`.

Deviations: none yet.

**Toolkit**

- Tool: `CLAUDE.local.md` and `AGENTS.md`, which I used to choose the base class and helpers and to decide what the spider should leave to the pipelines, then `plan-check`, which I used to grade the plan against the real API data and the repository's helpers before building from it.
- What it produced: the plan above, covering one new file and the existing helpers it reuses, with a list of what stays unchanged. `plan-check` ultimately returned `accept` after validating the plan against the live dealer data and the helper code.
- Difficulties: the conventions settle most of the structure, but not two data questions: what to do with inactive dealers, and whether to make one location per dealer or one per department. `plan-check` also exposed problems the first plan missed: the department data is nested, the opening hours are irregular, some dealers have no `Website` value, and `apply_category` overwrites the `shop` tag, so applying three shop categories to one item keeps only the last.
- What changed: I decided both data questions: no early filtering of inactive records (the output will show whether conventions exclude them), and one location per dealer unless the departments have separate addresses or coordinates. Because of `plan-check`'s findings, the plan moved to explicit field mapping instead of relying on `DictParser`, guarded access to `Website`, `add_ranges_from_string` for the irregular hours, and a deliberate one-feature-per-dealer category strategy that accounts for `apply_category` overwriting `shop`.

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
