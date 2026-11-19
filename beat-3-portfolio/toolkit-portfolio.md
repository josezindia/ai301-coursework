# Toolkit Portfolio

Path: `beat-3-portfolio/toolkit-portfolio.md`

**Student:** Jose

## Featured Tools

I built a shortlist of candidate issues from the Open Source Issue Tracker, then used my customized `issue-scout` to evaluate which ones were realistic and matched my interests in Python, backend work, APIs, infrastructure, and automation rather than frontend-heavy changes. From there, I treated each tool as a separate safety check: `repro-check` confirmed the problem was real, the ship-gate made sure the repository's own checks passed before I opened the PR, and the PR monitor kept me from replying to a bot comment that needed no answer.

**issue-scout**

**What it is, and the judgment hole I wrote:** I used its rubric and search strategy to evaluate and re-grade a shortlist of candidate issues I had built from the Open Source Issue Tracker. I updated the skill's scope and search strategy to reflect my Python, backend, infrastructure, automation, and AI systems interests. My fit profile says:

> I prefer issues that are clearly scoped and reproducible, especially those involving Python, backend systems, infrastructure, cloud, Windows/Linux environments, CI/CD, GitHub Actions, or automation. I would like to avoid issues that depend heavily on frontend design work, obscure languages, or large architectural changes.

**How it evolved, dated:**

- 2026-09-28 (Unit 1, `issue-select`): I revised the `Newcomer-sized scope` check because my first version was too strict and rejected issues just because they involved several related files or had a long description. I revised it to focus on concrete warning signs instead: umbrella scope, support-only questions, unresolved design work, core-internal changes, or a long history of failed contribution attempts.
- 2026-11-15 (Unit 5, commit `56a33c3`): Some searches returned weak, already-claimed, or overly broad issues, and one query initially returned no useful results. I refined the search query and ranking approach, documented the problems in `scan-notes.md`, and reran the scout with improved results. The original scope check could reject bounded issues simply because they contained an open design edge, so I replaced the old newcomer-scope check with a `scope-feasible` check so open design questions increase difficulty instead of automatically causing rejection, then re-ran the scout before choosing the issue.

**Evidence:**

- Unit 1 run history: `16/20 → 1/4 targeted → 1/1 issue-01 → 1/1 issue-19 → 1/1 issue-15 → 3/3 targeted → 19/20 full run → 18/20 final saved run.`
- Before (Unit 1 rubric row): `Newcomer-sized scope | ... | Pass unless the issue is explicitly an umbrella or tracking issue, is purely a usage/support question, has unresolved design debate with no maintainer decision, or a maintainer states that the fix requires changes to core internals. ... | required`
- After (Unit 5): a `scope-feasible` check, so open design questions increase difficulty instead of automatically causing rejection.
- Result: after I built a shortlist of candidate issues and updated the scout's scope-feasibility rule, `alltheplaces/alltheplaces` issue #8510 returned an `accept` verdict with a straightforward difficulty rating. The issue asks for an Australian Isuzu UTE spider backed by an official dealer API.
- Claim comment: https://github.com/alltheplaces/alltheplaces/issues/8510#issuecomment-6909757445

**repro-check**

**What it is, and the judgment hole I wrote:** `repro-check`, which I used to grade my reproduction against the issue before posting. My rubric has five checks: Environment recorded, Reproduction steps, Behavior matches issue, Honest outcome, and Repository communication. I made all five checks required because each proof family can prevent a bad reproduction package from being posted. A failing command alone is not enough. The evidence must show the same behavior described by the issue rather than a nearby or unrelated failure.

**How it evolved, dated:**

- 2026-10-04 (Unit 2, commit `3fe2478`): The first full eval scored 18/20 overall, but the disclosure category floor was not met. My original communication rule was too general, so I refined it to distinguish between general code or PR guidance and requirements that explicitly apply to issue comments. After that change, pkg-20 was correctly rejected while packages without comment-specific disclosure requirements could still pass.
- 2026-11-16 (Unit 6, commits `313486e` and `cd7fbe4`): The initial reproduction used plain `curl`, which returned HTTP 403 even though the repository's Scrapy client could access the endpoint. I reproduced with the project's own Scrapy client instead of `curl`, and recorded the 3 `IsActive: False` records so the final location count can be explained.

**Evidence:**

- Unit 2 run history: `First full eval: 18/20 ... Targeted rerun of pkg-05, pkg-12, pkg-13, and pkg-20: 4/4. Final full saved run: 19/20.`
- Before: a plain `curl` request to `https://www.isuzuute.com.au/isuzuapi/Dealer/GetAllDealers` returns HTTP 403.
- After: `uv run scrapy fetch "https://www.isuzuute.com.au/isuzuapi/Dealer/GetAllDealers"` returns HTTP 200 and a `Dealers` array of 165 records, three of which are marked `IsActive: False`.
- Example dealer record from the API:

```json
{
  "Code": "7207",
  "Name": "Blacklocks Isuzu UTE",
  "State": "New South Wales",
  "PostCode": "2641"
}
```

- `locations/spiders/isuzu_au.py` does not exist at commit `7f884e7c743256597d197ea7dc1af88a8b257309`, so issue #8510 remains actionable.

**ship-gate**

**What it is, and the judgment hole I wrote:** I built `ship-manifest.md` from the checks the repository itself requires: `uv sync --frozen` from the README setup, `uv run pytest tests/test_item_attributes.py -q` from `AGENTS.md`, and `uv run pre-commit run --all-files` from the pre-commit and CI configuration.

**How it evolved, dated:**

- 2026-11-16 (Unit 7, commit `63b2dfc`): The work that took care was tracing each check to a repository source instead of guessing; I also found no checks I needed to exclude, so the excluded table stays empty.

**Evidence:**

```text
uv sync --frozen
uv run pytest tests/test_item_attributes.py -q
uv run pre-commit run --all-files
```

- On `feat/8510-isuzu-au`, the ship-gate ran every manifest check and passed, and `pr-precheck` then passed the branch, title and description.
- Opened PR #20576, "Add isuzu_au spider (165 locations)": https://github.com/alltheplaces/alltheplaces/pull/20576

**pr-monitor**

**What it is, and the judgment hole I wrote:** I installed and used the PR monitor on PR #20576 and wrote an `alltheplaces`-specific playbook. In my playbook I set a 3-day human-review quiet period based on recent merged `alltheplaces` contributor PRs, and bot comments such as the scraper-bot's CI report do not reset that clock.

**How it evolved, dated:**

- 2026-11-16 (Unit 8, commit `63b2dfc`): The monitor first classified the bot's CI comment as FEEDBACK-IN, as if it were maintainer feedback needing a response, even though nothing in it asked for a change. I kept the draft off the PR thread, and my playbook now sets a 3-day human-review quiet period, based on recent merged contributor PRs, that bot comments do not reset.

**Evidence:**

```text
isuzu_au.py
165 items
Resulted in a success ✅
```

- The scraper-bot ran the spider on the PR and reported a `success` with 165 items, matching my local run.
- The PR monitor classified this as FEEDBACK-IN, but its drafted move was to not reply and wait for human review, which I agreed with.
- November 16, 2026: the PR initially had no human maintainer feedback, so I waited instead of posting an unnecessary reply. Later that day, PR #20576 was merged into `alltheplaces:master`.

## Inventory

- Voice guide: my rules for upstream comments; I only claim what my commands, logs, tests, or other artifacts actually demonstrate.
- Voice guard: I installed and tested the voice guard so outgoing GitHub comments are checked against my communication rules before posting.
- `CLAUDE.local.md` / repo memory: a local `CLAUDE.local.md` for `alltheplaces` with the repository conventions, relevant Isuzu spider patterns, and required test commands; I treat `isuzu_th.py` as a reference for brand attributes and categories only.
- `plan-check`: I used it to grade the plan against the real API data and the repository's helpers before building from it; it caught the data-shape and `apply_category` problems before I wrote code, and ultimately returned `accept`.
- `pr-precheck`: in Unit 4 I kept commit hygiene as `preferred` rather than `required`, and the final full run reached 18/20; in Unit 7 it passed the branch, title and description before I opened PR #20576.

## The Arc

**Beat 1**
Date: November 2026 (Unit 5, 2026-11-15)
Friction: Some searches returned weak, already-claimed, or overly broad issues, and the original scope check could reject bounded issues simply because they contained an open design edge.
Change: I refined the search query and ranking approach, documented the problems in `scan-notes.md`, and replaced the old newcomer-scope check with a `scope-feasible` check so open design questions increase difficulty instead of automatically causing rejection.
Result: After I built a shortlist of candidate issues, issue #8510 received an `accept` verdict with a straightforward difficulty rating, and I chose it.

**Beat 2**
Date: November 2026 (Unit 6, 2026-11-16)
Friction: The initial reproduction used plain `curl`, which returned HTTP 403 and made the API look unavailable when it was not.
Change: I changed the reproduction to use the project's own Scrapy client instead of `curl`.
Result: Scrapy returned HTTP 200 and the dealer response contained 165 records, and I confirmed that `locations/spiders/isuzu_au.py` does not exist.

**Beat 3**
Date: November 2026 (Unit 7, 2026-11-16)
Friction: The work that took care was tracing each check to a repository source instead of guessing.
Change: I built `ship-manifest.md` from the checks the repository itself requires and ran the ship-gate on my real branch before opening the PR.
Result: On `feat/8510-isuzu-au`, the ship-gate ran every manifest check and passed, `pr-precheck` then passed the branch, title and description, and I opened PR #20576.

**Beat 4**
Date: November 2026 (Unit 8, 2026-11-16)
Friction: The monitor read the scraper-bot comment on PR #20576 and initially classified the PR as FEEDBACK-IN, as if it were maintainer feedback needing a response.
Change: In my playbook I set a 3-day human-review quiet period, and bot comments such as the scraper-bot's CI report do not reset that clock.
Result: I kept the draft off the PR thread and did not reply unnecessarily; later that day, PR #20576 was merged into `alltheplaces:master`.

## Limitations and What's Next

[PR #20576](https://github.com/alltheplaces/alltheplaces/pull/20576) has now been merged, so the contribution itself is complete. The toolkit still depends on my judgment: issue-scout can help evaluate whether an issue is feasible, but it cannot know whether I will actually enjoy working on it; repro-check can verify the behavior I reproduced, but not every possible edge case; ship-gate can prove the required checks passed, but not that every design choice is the best one; and pr-monitor can still misread context, especially bot activity. If I continued improving the toolkit, I would make the tools share more context with each other and make issue discovery more focused on infrastructure, platform engineering, Linux, cloud, and automation work.

**Tool limits.**

issue-scout: Some searches returned weak, already-claimed, or overly broad issues, and one query initially returned no useful results.
repro-check: The endpoint's response depended on the HTTP client, so a plain `curl` request made the API look unavailable when it was not.
ship-gate: Tracing each check to a repository source instead of guessing took care.
pr-monitor: The monitor first classified the bot's CI comment as FEEDBACK-IN, as if it were maintainer feedback needing a response, even though nothing in it asked for a change.
