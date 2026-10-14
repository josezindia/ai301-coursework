# Procedure: how this tool grades a PR package

## Read order

1. **Live mode only:** read `scope.md` first. If the `Repo:` line is
   still a placeholder, stop and tell the student to fill it. If the
   PR targets a different repo, refuse. Otherwise note the house rules,
   then read `voice-guide.md` and note its rules. In eval mode, skip
   this step entirely.
2. Read the **issue** and **thread highlights**. Note the reported
   behavior and any explicit maintainer direction (a requested
   approach, a question, a stated preference).
3. Read the **repo facts**. Write down each stated ask as its own
   line: required template sections, checklist items, required entries
   (changelog or whatsnew), and the AI-use policy. If the repo states
   none of these, write "no stated asks".
4. Read the **plan context** before you open the PR, and write down:
   - **Planned steps:** each change the plan says it will make.
   - **Planned files.**
   - **Not in scope:** each item, listed separately.
   - **Deviation or deferral notes:** what is deferred and why.
   - **Test plan items:** each repro, named failure mode, control
     case, and suite or check, listed separately.
5. Only then read the **candidate PR**, in this order: diff, commits,
   test evidence, and finally title and description.

The order matters. The plan comes first so the diff is judged against
what was promised, not against what the description says was done. The
description comes last so its claims are checked against a diff you
have already read, rather than steering how you read the diff.

## Evidence gathering

Build four side-by-side comparisons. Write each one down before grading
anything.

1. **Plan vs diff** (plan-fidelity, diff-reviewability). Make a hunk
   table: one row per hunk, with columns `file | what it changes | maps
   to`. Fill "maps to" with one of: a planned step, a deviation note, a
   required companion (a test, fixture, snapshot, or changelog entry the
   repo asks for), "unplanned", "out-of-scope item", or "debris". Then
   go through the list of planned steps and mark each one `present` (name
   the hunk), `deferred` (quote the note), or `missing`.
2. **Tests vs test plan** (test-evidence). For each test plan item,
   record `shown` (quote the before or expected-after and the after),
   `disclosed skip` (quote the disclosure), or `absent`. Note which code
   path each shown run exercises: the changed path, or an unchanged or
   control path. Record whether the repo suite or checks outcome appears,
   and quote it. Then list each failure mode the title or description
   claims is fixed, and check that each one has a `shown` row.
3. **Description vs diff** (description-fidelity). Pull out every
   factual claim from the title and description, one per line. Mark each
   `true` (name the hunk that bears it out), `false` (name the hunk that
   contradicts it, or note that the claimed change does not exist), or
   `unverifiable`. Then check that every deferral from read step 4 also
   appears in the description.
4. **Standards vs PR draft** (repo-standards). For each ask from read
   step 3, record `met` (quote where), `missing`, or `boilerplate`. For
   required entries such as a changelog or whatsnew, look for them in
   the diff, not just in the checklist text. If the AI policy requires
   disclosure, look for a disclosure statement in the description. In
   live mode, also check each scope house rule.

For the preferred checks: copy the commit messages as written, and note
whether the description responds to each maintainer direction from read
step 2.

## Check execution

1. Run the checks in rubric order: plan-fidelity, description-fidelity,
   test-evidence, diff-reviewability, repo-standards, commit-hygiene,
   thread-engagement. Grade every check, even after a required check has
   failed. Eval mode always grades the complete package.
2. Grade each check only from its comparison in Evidence gathering,
   using the rubric's pass condition as written:
   - plan-fidelity fails on any `unplanned` or `out-of-scope item` row,
     or on any `missing` planned step.
   - description-fidelity fails on any `false` claim, or on a deferral
     the description leaves out.
   - test-evidence fails on any claimed fix with no `shown` row, on
     evidence that exercises only an unchanged path, or when no suite or
     checks outcome is reported.
   - diff-reviewability fails on any `debris` row.
   - repo-standards fails on any `missing` or `boilerplate` ask.
3. A hunk can count against more than one check. For example, unrelated
   reformatting is both `unplanned` and `debris`. Grade it under every
   check whose condition it meets.
4. When the package does not contain the evidence a check needs, grade
   the check `unclear` and say what is missing. The one exception is
   repo-standards: when there is no stated ask, that is a `pass`, not
   `unclear`.
5. Write the evidence line as a single line naming the deciding fact:
   the hunk, the quote, or the missing item. Never write "looks fine".
6. Do not re-read the whole package to grade a check. Go back to the
   source only when a comparison row is ambiguous, and only to the part
   that row came from.
7. If a step needed for grading is not covered here, report it as a
   procedure gap in the summary. Do not invent a new step.

## Verdict assembly

1. Apply the rubric's verdict rule: `accept` only if all five required
   checks pass. A `fail` or `unclear` on any required check gives
   `reject`. Preferred checks never change the verdict.
2. The **deciding check** for a reject is the first required check that
   failed or was unclear, in rubric order. Lead the readable summary with
   that check's evidence line. For an accept, lead with the plan-fidelity
   evidence and note any disclosed deferral.
3. In live mode, add the voice-guide findings to the summary as
   advisory notes. They do not change the verdict.
4. End with the contract's JSON block. Use the bundle id in eval mode or
   the PR URL in live mode. List all seven checks in rubric order. Nothing
   may follow the block.
