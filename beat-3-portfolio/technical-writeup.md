# Adding Isuzu UTE Australia to All the Places: My First Open-Source Spider

## Overview

All the Places is an open-source project that collects store and dealer locations from official brand websites and publishes them as open geodata. Each brand gets a small scraper, called a "spider." The project already covered Isuzu, but it was missing the Isuzu UTE dealer network in Australia. I wrote a spider that pulls those dealers from Isuzu UTE Australia's official API and turns each dealer into one clean location record. Run against the live site, it returned all 165 locations. My pull request is open and waiting for a maintainer to review it.

## What happened

### Finding the issue

Finding an issue was harder than I expected. My background is infrastructure: Linux and Windows servers, file systems, backup storage, VMware. Most beginner-friendly open-source issues I found were front-end fixes or documentation changes, which have little to do with what I actually do. I couldn't find many issues close to infrastructure, Linux or cloud work.

This one still appealed to me for a few reasons. It meant investigating a real API and working out how its data was structured before writing any code. It was an automation job, which is close to my day-to-day work. It needed careful data validation, because a wrong location is worse than a missing one. And it made me work in a codebase I'd never seen, with its own conventions and tooling, which was a skill I wanted to practice.

### Understanding the source

Isuzu UTE Australia publishes its dealer list through an official API. It returned **165 dealer records**. Before writing anything, I read through the records to learn their shape.

Three things stood out:

- **Departments.** Each dealer record is broken into Sales, Service and Parts sections, and each section has its own details such as contact info and opening hours.
- **Irregular hours.** The opening hours didn't follow one consistent format. I found variations such as en dashes, 8.30, 5:00PM, and trailing dashes.
- **Unexpected combinations.** A few records had status flags and data that did not match what I expected. For example, dealer 7242 had `IsActive: False` at the top level but still had a valid Sales department with usable location data.

### Building the spider

I built the spider on **`JSONBlobSpider`**, an existing base class in the project for sources that return a JSON list of records. That kept my code small, because the base class already does the fetching and looping.

I reused the **brand metadata the project already had for Isuzu** instead of defining it again, so these dealers link to the same brand entry as the rest of Isuzu's data.

I made one design decision early: **one dealer becomes one feature**. Sales, Service and Parts at the same address all become a single location, not three.

Because each department has its own details, I needed a rule for which department to take contact info and hours from. I used a **fallback order of Sales, then Service, then Parts**. If Sales has the data, the spider uses it. If not, it tries Service, then Parts. That way a dealer with no Sales department still gets complete details.

### Proof 1: local test

```text
finish_reason: finished
165 features
165 unique refs
no errors

uv run pytest tests/test_item_attributes.py -q
3 passed
```

### A wrong assumption I corrected

Dealer **7242** caught me out.

At first I assumed dealer 7242 would need to fall back to Service because I did not expect it to have a valid Sales block.

When I checked the raw API response for that dealer, it showed that 7242 did have a Sales block with usable dealer information, even though its top-level `IsActive` value was `False`.

So I changed the plan by correcting that assumption and keeping the normal Sales, then Service, then Parts fallback. I also did not filter the dealer out based only on `IsActive`, because I could not verify what that undocumented flag meant. The lesson was to check the raw record before deciding what a field means, especially when the field isn't documented.

### Proof 2: repository result

The project's automated scraper bot ran my spider against the live source and reported success:

```text
isuzu_au.py
165 items
Resulted in a success ✅
```

### Testing before the PR

Before opening the pull request, I ran the project's checks:

- **pytest**: 3 tests passed
- **pre-commit**: passed (formatting and lint rules)
- **ship-gate**: passed

## Limitations and what's next

- **Still waiting on review.** The PR is open and no maintainer has looked at it yet, so the design could still change.
- **One feature vs. per-department.** I combined each dealer into one feature. A maintainer might want separate features for Sales, Service and Parts, especially where departments have different hours. If they do, the spider's structure makes that a fairly contained change.
- **`IsActive` is undocumented.** The API has a top-level `IsActive` flag, and I couldn't find any documentation on what it means. I didn't want to drop dealers based on a guess, so this is an open question I've raised for review.
- **What I'd do differently.** Next time, I would inspect the full API structure and the closest existing spiders earlier before locking in assumptions about how the records should map into the project.
- **If maintainers request changes.** I'll make the changes on the same branch, re-run pytest, pre-commit and ship-gate, and reply to each review comment so the reviewer can see what I changed and why.
