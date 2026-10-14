# Procedure: how this skill grades a plan package

## Read order

1. Read the issue context and thread highlights first. Note the reported behavior, constraints, prior findings, and any communication requirements that apply to plan comments.
2. Read the reproduction evidence next. Record the exact observed behavior, the commands or inputs that produced it, the expected behavior, and any limits on what was actually proven.
3. Read the plan after the evidence. Note the diagnosis, scope, exclusions, files to touch, implementation approach, test plan, risks, and unknowns.
4. Read the draft plan comment last. Compare it with the plan and issue thread so the comment is judged in context.
5. Read the repo-facts block and contribution guidance whenever a check depends on repository structure, testing conventions, or communication rules.

## Evidence gathering

1. For diagnosis, place the plan's stated cause or hypothesis next to the reproduction evidence that supports or contradicts it.
2. For scope, record what the plan says will change, which files it names, and what it explicitly says will not change.
3. For implementation approach, identify the code path, function, file, configuration, or behavior the plan proposes to change and compare it with the reproduced failure.
4. For executability, record whether the plan gives enough technical detail for someone to know where to begin and what sequence of work to follow.
5. For the test plan, record the before-fix reproduction steps, proposed after-fix commands, expected observable result, and any regression checks.
6. For risks and unknowns, record each uncertainty, assumption, and unresolved question, then note whether the plan proposes a way to investigate any material unknown.
7. For communication, compare the draft plan comment with the plan, issue thread highlights, repo-facts block, and contribution rules that explicitly apply to issue comments.

## Check execution

1. Grade `Diagnosis matches evidence` first because later checks depend on whether the plan is solving the reproduced problem.
2. Grade `Scope is bounded` next. Fail it if the plan expands into unrelated fixes, cleanup, refactors, or adjacent behavior that the reproduced issue does not require.
3. Grade `Approach targets the cause` by comparing the proposed implementation with the diagnosis and reproduction evidence.
4. Grade `Plan is executable` by asking whether another contributor could start the work from the named files and steps without inventing a missing technical decision.
5. Grade `Test plan proves the change` by checking whether the proposed tests exercise the same real-code behavior that failed before and state an observable expected result after the fix.
6. Grade `Risks and unknowns are honest` by checking for a material uncertainty, limitation, or confidence claim that the plan misrepresents. Explicitly acknowledged risks, testing limits, deferred questions, and details to be confirmed during implementation pass; do not require the author to invent additional unknowns.
7. Grade `Comment fits thread and repo` last because it depends on the completed understanding of the plan and repository context.
8. If evidence for a required check is missing or too ambiguous to decide, grade that check `unclear`. Do not infer missing facts.
9. Reuse gathered evidence for related checks unless two sources conflict or more context is needed.

## Verdict assembly

1. Collect the grade for every rubric check.
2. Accept only if every required check passes.
3. Treat `unclear` on any required check as a failure, so the final verdict is reject.
4. If the verdict is reject, identify the deciding failed or unclear required check and quote the smallest relevant evidence needed to explain why.
5. If the verdict is accept, briefly state that every required check passed and cite the strongest evidence for diagnosis, scope, and test-plan readiness.
6. End with the required JSON block using only the binary verdict `accept` or `reject`.
