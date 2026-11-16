# Contribution: Isuzu Ute (shop/car, shop/car_repair, shop/car_parts) (AU) (165 locations)

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Jose
**GitHub Username:** josezindia
**Issue:** https://github.com/alltheplaces/alltheplaces/issues/8510
**Pull Request:** https://github.com/alltheplaces/alltheplaces/pull/20576

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

**Unit 7: Wrote a gate manifest for `alltheplaces` and ran the ship-gate on my real branch before opening the PR.**

I built `ship-manifest.md` from the checks the repository itself requires: `uv sync --frozen` from the README setup, `uv run pytest tests/test_item_attributes.py -q` from `AGENTS.md`, and `uv run pre-commit run --all-files` from the pre-commit and CI configuration. On `feat/8510-isuzu-au`, the ship-gate ran every manifest check and passed, and `pr-precheck` then passed the branch, title and description. The work that took care was tracing each check to a repository source instead of guessing; I also found no checks I needed to exclude, so the excluded table stays empty.

**Unit 8: Installed and used the PR monitor on PR #20576 and wrote an `alltheplaces`-specific playbook.**

The monitor read the scraper-bot comment on PR #20576 and initially classified the PR as FEEDBACK-IN, but its drafted move correctly recommended no reply and waiting for human review. I kept the draft off the PR thread. In my playbook I set a 3-day human-review quiet period based on recent merged `alltheplaces` contributor PRs, and bot comments such as the scraper-bot's CI report do not reset that clock.

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

Deviations: none to the design. During implementation, dealer `7242` turned out to be `IsActive: False` while still carrying Sales data; it is recorded in Implementation Notes and did not change the plan.

**Toolkit**

- Tool: `CLAUDE.local.md` and `AGENTS.md`, which I used to choose the base class and helpers and to decide what the spider should leave to the pipelines, then `plan-check`, which I used to grade the plan against the real API data and the repository's helpers before building from it.
- What it produced: the plan above, covering one new file and the existing helpers it reuses, with a list of what stays unchanged. `plan-check` ultimately returned `accept` after validating the plan against the live dealer data and the helper code.
- Difficulties: the conventions settle most of the structure, but not two data questions: what to do with inactive dealers, and whether to make one location per dealer or one per department. `plan-check` also exposed problems the first plan missed: the department data is nested, the opening hours are irregular, some dealers have no `Website` value, and `apply_category` overwrites the `shop` tag, so applying three shop categories to one item keeps only the last.
- What changed: I decided both data questions: no early filtering of inactive records (the output will show whether conventions exclude them), and one location per dealer unless the departments have separate addresses or coordinates. Because of `plan-check`'s findings, the plan moved to explicit field mapping instead of relying on `DictParser`, guarded access to `Website`, `add_ranges_from_string` for the irregular hours, and a deliberate one-feature-per-dealer category strategy that accounts for `apply_category` overwriting `shop`.

---

## Testing Strategy

**Before and after**

- Before: at commit `7f884e7c743256597d197ea7dc1af88a8b257309`, `locations/spiders/isuzu_au.py` does not exist, so the project emits no Isuzu UTE Australia locations, while the dealer API returns 165 records in `Dealers`.
- After: on branch `feat/8510-isuzu-au`, `uv run scrapy runspider locations/spiders/isuzu_au.py -o out.geojson -L INFO` produces exactly **165 features**, one per record in the API. The crawl stats show `atp/brand/Isuzu: 165`, `atp/brand_wikidata/Q29803: 165`, `atp/nsi/match_perfect: 165`, `atp/country/AU: 165`, and a category split of `atp/category/shop/car: 154` and `atp/category/shop/car_repair: 11`.

**Representative records**

I read individual records rather than relying on the count, picking dealers that exercise each branch of the category logic:

| Ref | Dealer | Source data | Output checked |
|---|---|---|---|
| `7207` | Blacklocks Isuzu UTE, Lavington NSW | Active; Sales, Service and Parts | `shop=car`, `service:vehicle:car_repair=yes`, `service:vehicle:car_parts=yes`; address "Corner Kaitlers Rd and Wagga Rd", postcode 2641; phone `+61 2 6049 5500`; website present; hours `Mo-Fr 08:00-17:00; Sa 09:00-12:30` |
| `7236a` | Victory Isuzu UTE - Service and Parts, South Murwillumbah NSW | Active; no Sales, has Service and Parts | `shop=car_repair`, `service:vehicle:car_parts=yes`; address and coordinates taken from the Service department; no `Website` in the data, and the spider emits the item without a website instead of failing; hours `Mo-Fr 07:30-17:00` |
| `7242` | Bathurst Isuzu UTE, Kelso NSW | Top-level `IsActive: False`, but has Sales, Service and Parts | `shop=car`, `service:vehicle:car_repair=yes`, `service:vehicle:car_parts=yes`; address "202 Sydney Road", postcode 2795; hours `Mo-Fr 08:30-17:30; Sa 08:30-15:00` |
| `7248` | Cardiff Isuzu UTE, Cardiff NSW | Active; Sales, Service and Parts | `shop=car`, repair and parts extras `yes`; hours across three day groups, `Mo-Fr 08:00-17:30; Sa 08:30-17:30; Su 09:30-17:00`, confirming `add_ranges_from_string` handles the irregular hours text |

**Repository checks**

- `uv run pytest tests/test_item_attributes.py -q`: **3 passed**, so the Isuzu brand and Wikidata values match NSI.
- `uv run pre-commit run --files locations/spiders/isuzu_au.py`: **passed** (formatting, import sorting and lint hooks all clean).

**Toolkit**

- Tool: Scrapy's `runspider` through `uv` to build the GeoJSON output and crawl stats, a short Python script to pull the four representative records out of the output and compare them with the saved API response, then the repository's `pytest` and `pre-commit` checks named in `CLAUDE.local.md`.
- What it produced: 165 features matching the 165 API records, stats confirming brand, Wikidata, NSI and country on every item, field-level confirmation for `7207`, `7236a`, `7242` and `7248`, 3 passing tests, and a clean pre-commit run.
- Difficulties: the feature count alone could not show whether categories, extras, missing websites or irregular hours were handled correctly, and the three `IsActive: False` records made it unclear at first whether 165 was the right number.
- What changed: I chose records by what they exercise (full dealer, service-only dealer with no website, inactive dealer, multi-group hours) and checked each one against its source record, instead of only checking the item count.

---

## Implementation Notes

**2026-11-16**

Accepted the plan: after revising it for nested department data, irregular hours, missing `Website` values and `apply_category` overwriting `shop`, `plan-check` returned `accept`, and I built from that version.

**2026-11-16**

Created `locations/spiders/isuzu_au.py` on branch `feat/8510-isuzu-au` as a `JSONBlobSpider` reading the `Dealers` array, with `item_attributes = ISUZU_SHARED_ATTRIBUTES`. As planned after `plan-check`, the fields are mapped explicitly rather than through `DictParser`: the spider takes the first department present (Sales, then Service, then Parts) and reads address, coordinates, phone, email and website from it, using the dealer `Code` as `ref`. `Website` is read only when present, because some dealers (for example `7236a`) have none.

Categories follow the one-feature-per-dealer strategy. Because `apply_category` overwrites `shop`, each dealer gets a single `shop` value: `shop=car` when it has Sales data, otherwise `shop=car_repair`. Repair service on a sales dealer and parts sales are recorded with `apply_yes_no` extras (`service:vehicle:car_repair`, `service:vehicle:car_parts`) instead of competing `shop` tags. Opening hours are built by passing each `"<day> <hours>"` string to `OpeningHours.add_ranges_from_string`.

**2026-11-16**

Ran the spider and inspected the output: 165 features, matching the 165 API records, so nothing was dropped and the pipelines did not exclude the `IsActive: False` records. Small plan correction: dealer `7242` (Bathurst Isuzu UTE) is marked `IsActive: False` at the top level but still has full Sales, Service and Parts data, so under the Sales rule it is tagged `shop=car`. The plan had not considered that an inactive record could still carry sales data. I kept the planned behaviour of not filtering inactive records early. This correction did not change the overall design: still one new file, one feature per dealer, and the same category rules. Then ran `pytest` (3 passed) and `pre-commit` (passed) on the new file.

**2026-11-16**

Ran the ship-gate against `ship-manifest.md` on `feat/8510-isuzu-au`: `uv sync --frozen`, the item-attribute tests and `pre-commit run --all-files` all passed, and `pr-precheck` passed the branch, title and description. Opened PR #20576, "Add isuzu_au spider (165 locations)".

**2026-11-16**

The scraper-bot ran the spider on the PR and reported a `success` with 165 items, matching my local run. The PR monitor classified this as FEEDBACK-IN, but its drafted move was to not reply and wait for human review, which I agreed with. I kept the draft off the PR thread and am waiting out the 3-day human-review quiet period from my playbook; the bot comment does not reset it.

**Toolkit**

- Tool: Claude Code with my `CLAUDE.local.md` repo memory to write the spider against the plan, then Scrapy `runspider`, a Python script comparing output records with the saved API response, `pytest` and `pre-commit` to check the result.
- What it produced: the single new spider file, a 165-feature output, the `7242` finding, and passing test and lint runs.
- Difficulties: the `IsActive` flag and the department data disagree for `7242`, which the plan did not anticipate, and `apply_category`'s overwrite of `shop` meant the three categories in the issue could not all be `shop` tags on one feature.
- What changed: I recorded the `7242` correction here instead of adding special-case filtering, and used `shop=car` or `shop=car_repair` plus yes/no extras for repair and parts, so the design from the plan stayed the same.

---

## Pull Request

**PR Link:** https://github.com/alltheplaces/alltheplaces/pull/20576

**PR Title:** Add isuzu_au spider (165 locations)

**PR Description:** The PR adds the `isuzu_au` spider for Australian Isuzu UTE dealers, reading the official dealer API and producing 165 locations.

**Maintainer Feedback:**

- November 16, 2026: no human maintainer feedback yet.
- The scraper-bot ran the spider successfully and reported 165 items.
- I did not reply because the bot reported success and there was no maintainer question or requested change.

**Status:** Awaiting review

**Toolkit**

- Tool: my `ship-gate` with the gate manifest, then the `pr-precheck` skill on the branch, PR title and description before opening the PR; after opening it, the PR monitor with my `alltheplaces` playbook.
- What it produced: `ship-gate` passed all manifest checks and `pr-precheck` passed its checks before the PR was opened. The PR monitor read the scraper-bot's 165-item success report and drafted a "no reply, wait for human review" move.
- Difficulties: the monitor first classified the bot's CI comment as FEEDBACK-IN, as if it were maintainer feedback needing a response, even though nothing in it asked for a change.
- What changed: I kept the draft off the PR thread, and my playbook now sets a 3-day human-review quiet period, based on recent merged contributor PRs, that bot comments do not reset.

---

## Learnings and Reflections

**Technical Skills Gained**

I learned how an `alltheplaces` spider is put together: `JSONBlobSpider` for reading a JSON array, shared brand attributes like `ISUZU_SHARED_ATTRIBUTES`, `apply_category` and `apply_yes_no` for tags, and `OpeningHours.add_ranges_from_string` for messy hours text. The bigger lesson was to validate my assumptions against the real API data instead of guessing. I assumed `DictParser` would map the fields and that I could put three `shop` categories on one dealer; the actual response had nested department data, missing websites and irregular hours, and `apply_category` overwrites `shop`. I also learned to read crawl stats and individual output records together, since a matching count of 165 said nothing about whether categories or hours were right.

**Challenges Overcome**

The first surprise was the endpoint returning HTTP 403 to `curl`, which made the API look dead; reproducing with the project's own Scrapy client showed it was fine. The harder part was that repository conventions mattered as much as code correctness. A spider that ran was not enough: it had to reuse the shared Isuzu attributes, follow the category conventions, and leave cleanup to the pipelines. What got me through was treating each tool as a separate safety check. `repro-check` confirmed the problem was real, `plan-check` caught the data-shape and `apply_category` problems before I wrote code, the ship-gate made sure the repository's own checks passed before I opened the PR, and the PR monitor kept me from replying to a bot comment that needed no answer.

**What I'd Do Differently Next Time**

I would inspect the raw data shape and the closest repository examples much earlier. Most of the plan revisions came from things I could have seen by reading a few full API records and a couple of similar spiders before writing the first plan, instead of having `plan-check` point them out. I would also look up the project's review pace before opening the PR, so I know from the start how long to wait before following up.

---

## Resources Used

- `alltheplaces` `AGENTS.md` and repository spider examples (`isuzu_jp.py`, `isuzu_th.py`, `templates/spiders/json_blob.tmpl`): the conventions, helpers and test commands the spider follows.
- Issue #8510: https://github.com/alltheplaces/alltheplaces/issues/8510, the request and the expected categories and location count.
- Official Isuzu UTE Australia dealer API: `https://www.isuzuute.com.au/isuzuapi/Dealer/GetAllDealers`, the data source I reproduced against and built from.
- CodePath `issue-scout`, `repro-check`, `plan-check`, `ship-gate` and PR-monitor materials: the tools and templates I used at each phase.
- GitHub PR #20576 and the scraper-bot CI output: https://github.com/alltheplaces/alltheplaces/pull/20576, which confirmed 165 items and a `success` run.
