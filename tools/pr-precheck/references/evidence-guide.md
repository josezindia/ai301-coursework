# Evidence guide: where evidence lives in a PR package

Each family below maps to a harness failure category: plan fidelity is
silent drift, test evidence is not tested, diff quality is unreviewable,
and standards and comms is the standards wall. A package that shows none
of the four failures is a clear accept.

## Plan fidelity (harness category: silent-drift)

**Where it lives.**
- *Eval:*
  - The `## Plan context` block: the plan's steps, its "Scope, one
    bounded change" statement, its `Files:` list, its "Not in scope:"
    list, and any deferral notes.
  - The `### Diff` under `## Candidate PR`: the `--- a/` and `+++ b/`
    file headers and each `@@` hunk.
  - The `### Title` and `### Description` sections, for fidelity claims.
- *Live:*
  - `plan.md`, including `## Deviations`.
  - The branch diff, from `git diff main...HEAD` (`--stat` for the file
    list).
  - The title and description in `pr_draft.md`.

**What good looks like.**
- Each changed hunk carries out a planned step, falls under a deviation
  note, or adds a companion the repo requires (tests, fixtures,
  changelog).
- Each planned step appears in the diff, or is deferred in a note that
  the description repeats.
- Every claim in the description can be pointed to in the diff.

**What silent drift looks like.**
- *Doing more than the plan:* a second file the plan never names, a
  rewrite of an unrelated function, or a new option, flag, or setting,
  especially one the plan lists as not in scope.
- *Doing less than the plan:* a planned step (a docs update, a second
  fix) missing from the diff, with no note explaining why.
- *A description that does not match the diff:* "implements the plan
  exactly" or "no changes beyond the plan" over unplanned hunks, or
  "docs now document X" when the diff touches no docs.

A polished description does not reduce drift. Check the description's
claims against the diff yourself.

## Test evidence (harness category: not-tested)

**Where it lives.**
- *Eval:*
  - The candidate PR's `### Test evidence` section.
  - In the plan context, the plan's "Test plan:" sentence (each repro,
    each failure mode named, each control case, and the suite) and the
    "Repro evidence" paragraph, which gives the original failing
    command and output.
  - Tests added in the diff support the evidence; they do not replace
    the shown runs.
- *Live:*
  - `test_evidence.md`, which holds the captured command output.
  - The test plan in `plan.md`.
  - The repo's test or CI command, as named in CONTRIBUTING.md.

**What decisive evidence looks like.**
- The issue's own repro command is re-run on the branch.
- The output shows the after behavior, next to the before output or the
  plan's stated expected-after. Example: "before: panicked (exit 101) /
  after: argument error, exit 2".
- Every failure mode the PR claims to fix has its own run.
- The control case is reported unchanged.
- The suite result is stated, such as "`cargo test` passes (614
  tests)".

**What is not decisive.**
- "tested locally", "works now", "verified on my machine", or
  "`cargo test` passes" alone.
- A run of the control or an unchanged path instead of the broken one.
- Only one of two failure modes the plan named.
- A new test that covers something other than the fix.
- "(none provided)".

## Diff quality (harness category: unreviewable)

**Where it lives.**
- *Eval:* the `### Diff` (every `+` and `-` line) and the `### Commits`
  list.
- *Live:* the full `git diff main...HEAD` and `git log --oneline
  main..HEAD`.

**What reviewable looks like.** Every changed line serves the fix or a
required companion. A reviewer can find the fix immediately.

**Debris to look for:**
- Leftover debug output: `eprintln!("DEBUG`, `dbg!`, `console.log`,
  `print(`.
- Commented-out code, or a commented-out first attempt.
- Dead helpers: `fn _unused…`, `#[allow(dead_code)]`, unused probes.
- Reformatting or re-indenting of lines the fix does not need, or
  identical lines removed and re-added.
- Import reshuffles that do no work.
- Drive-by refactors of unrelated code, such as collapsing assignments
  or rewriting an untouched function.
- Commit messages like "wip", "fix", "misc cleanups while debugging", or
  "gofmt" also suggest debris. They feed the preferred commit-hygiene
  check.

## Standards and comms (harness category: standards-wall)

**Where it lives.**
- *Eval:*
  - The `## Repo facts` block. Its `pull requests:` line gives the
    template and its sections or checklist. Its `contribution policy`
    line gives any stated AI-use policy and other asks.
  - The `## Thread highlights` block, for maintainer direction.
  - Where the PR meets those asks: the `### Description` (sections,
    checklist, issue link, disclosure) and the `### Diff` (a required
    changelog or whatsnew file actually present).
- *Live:*
  - The repo's `.github/pull_request_template.md` and
    `docs/CONTRIBUTING.md`.
  - The house rules in `scope.md`: a `fix/<issue>-<slug>` branch on a
    fork, one PR per issue, and the template always used.
  - The real issue thread.
  - `pr_draft.md`.

**What compliant looks like.**
- Every required template section has content specific to this PR.
- Every required checklist item is met. If an item asks for a file,
  that file is in the diff.
- If the policy requires AI disclosure, the description says how AI was
  used, in the author's own words. Treat every package as AI-assisted.
- If a maintainer gave direction in the thread, the description engages
  with it.
- If the repo states no template and no policy, there is nothing to
  wall against, so the check passes.

**What the wall looks like.**
- Missing "closes #" or checklist when the template requires them.
- A whatsnew or changelog entry that the policy requires but the diff
  lacks.
- Template headings filled with boilerplate.
- No AI disclosure under a policy that requires one.

Whether the description's claims match the diff belongs to plan
fidelity, above, not here.
