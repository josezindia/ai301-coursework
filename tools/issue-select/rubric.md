# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Maintainer activity | Repo facts: last 5 default-branch commits and maintainer first-response sample | Pass if at least one default-branch commit is within 90 days of the capture date OR a maintainer first-response sample shows a response within 30 days. | required |
| Repository in use | Repo facts: archived flag, latest release, and last push to any branch | Fail if the repository is archived. Otherwise pass if either the latest release or the last push occurred within 180 days of the capture date. | required |
| Newcomer-sized scope | Issue body, comment thread, issue open date, and linked PR history | Pass unless the issue is explicitly an umbrella or tracking issue, is purely a usage/support question, has unresolved design debate with no maintainer decision, or a maintainer states that the fix requires changes to core internals. Also fail if the issue has been open for more than 1 year and has at least 2 closed unmerged PRs or at least 3 clearly abandoned contributor attempts. Multiple related files, checklist items, or detailed acceptance criteria do not fail this check by themselves. | required |
| Already claimed | Repo facts: this issue assignees and linked PRs, plus the issue comment thread | Pass if there is no current assignee, no open linked PR, and no comment within 30 days of the capture date stating that someone is actively working on the issue, unless the thread clearly shows that attempt was abandoned. | required |
| AI contribution policy | Repo facts: contribution policy, dedicated AI policy files, and any linked contributor documentation or templates | Pass if the repository does not explicitly ban AI-assisted or AI-generated contributions. Disclosure, testing, understanding, or human-review requirements still pass. Silence also passes. | required |

## Verdict rule

Accept if every required check passes. Reject if any required check fails. Treat unclear as fail for required checks. Preferred checks, if added later, do not change the verdict and are used only to rank accepted issues.
