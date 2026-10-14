# Rubric: is this pull request ready to submit?

Five required checks cover the four failure families. Plan fidelity and
description fidelity together cover silent drift, test evidence covers
not tested, diff reviewability covers unreviewable, and repository
standards covers the standards wall. Two preferred checks record useful
observations but never change the verdict.

A disclosed shortfall is not a failure in itself. A PR that does less
than everything the issue asks for can be ready when the gap is stated:
the plan's deviation or deferral notes name it, the description repeats
it, and the description does not claim the deferred part is delivered.
What fails is an undisclosed gap, or a description that claims more or
less than the diff contains.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| plan-fidelity | Each hunk in the candidate PR's diff, read against the plan context's scope, files list, "not in scope" list, and any deviation or deferral notes. Live: `git diff main...HEAD` against `plan.md` and its `## Deviations`. | Pass when two things hold. First, each changed hunk carries out a planned step, falls under a deviation note, or adds a companion the repo's stated standards require (tests, fixtures, snapshots, a changelog or whatsnew entry). Second, each planned step appears in the diff or is explicitly deferred in the plan's notes or the description. Fail when the diff changes a file or behavior the plan never names, or one the plan scopes out, with no note. This includes new options, flags, settings, renames, and rewrites of unrelated functions. Also fail when a planned step is missing and nothing discloses the omission. | required |
| description-fidelity | Each claim in the PR title and description (what changed, what was not changed, "exactly as planned", "no changes beyond", "docs updated", "tests added", what is deferred), checked against the diff. | Pass when the diff bears out every factual claim in the title and description, and any deferral the plan records is stated in the description, not hidden. Fail when the description claims scope fidelity ("implements the plan exactly", "no functional changes outside X") over a diff that contains unplanned changes. Also fail when the description claims a change (docs, tests, a second fix) that the diff does not contain. | required |
| test-evidence | The candidate PR's test-evidence section, read against the plan's test plan (each repro and named failure mode), the repro evidence in the plan context, and any repo checks or suite the plan or repo names. Live: `test_evidence.md` against `plan.md`'s test plan. | Pass when three things hold. First, each failure mode the PR claims to fix is re-run through the changed code path, with observable output shown: an after result, plus a before or a stated expected-after. Second, the plan's control case, if it has one, is reported unchanged. Third, the outcome of the repo's test suite or named checks is reported. A test-plan item may go unrun only when the PR does not claim to fix it and the omission is disclosed. Fail on "tests pass", "works on my machine", or "verified locally" with nothing observable named. Fail when the evidence exercises only an unchanged or control path, when a named failure mode the PR claims to fix is never re-run, or when no evidence is given. | required |
| diff-reviewability | The unified diff's added and removed lines, hunk by hunk. | Pass when every hunk is part of the change or a required companion, and the diff contains no debris. Fail on any debris. Debris means: debug prints or logging left in (`eprintln!("DEBUG`, `console.log`, `print(`), commented-out code blocks or earlier attempts, dead functions or `allow(dead_code)` and `_unused` helpers, reformatting or re-indenting of lines the fix does not need, import reshuffles that do no work, duplicated churn lines, or drive-by edits to unrelated code. A single TODO that points to an issue the plan defers is not debris. | required |
| repo-standards | The repo-facts block's PR template asks, contribution policy, required checklist items, and AI-use policy, read against the PR title, description, and diff. Live: the repo PR template, `docs/CONTRIBUTING.md`, and the house rules in `scope.md`. | Pass when four things hold. First, every section the repo's PR template requires has content specific to this PR; "N/A" counts only where the template allows it and the reason is true. Second, required checklist items are met, such as an issue link, a changelog or whatsnew entry, or a tests box, and an entry the policy requires is actually in the diff. Third, if the repo's stated policy requires AI-use disclosure, the description contains one. Treat every package as AI-assisted work. Fourth, in live mode, the scope house rules are met. A repo with no template and no stated policy passes by default; a missing ask is not a failure. Fail when a stated template section, required checklist item, required entry, or required AI disclosure is missing or is placeholder boilerplate. | required |
| commit-hygiene | The commit list. | Pass when each commit message says what that commit changes ("cli: cap --threads at 4096"). Fail on messages like "wip", "fix", "misc", or "hopefully". | preferred |
| thread-engagement | Thread highlights from maintainers or the issue author, read against the description. | Pass when the description acknowledges or follows any explicit maintainer direction about the approach, or when the thread gives no such direction. Fail when the description ignores a maintainer's stated question or requested approach. | preferred |

## Verdict rule

The verdict is `accept` only if all five required checks (plan-fidelity,
description-fidelity, test-evidence, diff-reviewability, repo-standards)
grade `pass`. Any required check graded `fail` or `unclear` makes the
verdict `reject`. An unclear grade counts as a fail, because a claim the
package cannot verify is not ready to submit. Preferred checks
(commit-hygiene, thread-engagement) are graded and reported but never
change the verdict.

An honestly disclosed shortfall does not make a required check fail.
When the plan's notes and the description both state a deferral, and the
diff and evidence fully deliver everything else, the checks grade `pass`
and the package can be accepted.
