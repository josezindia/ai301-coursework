# Rubric: is this plan ready to post and build from?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Diagnosis matches evidence | The plan's stated cause read against the repro evidence, observed output, and issue context in the package | Pass if the diagnosis is a reasonable explanation of the reproduced behavior and does not contradict the available evidence. The exact root cause does not need to be fully proven before planning when the proposed approach includes a concrete way to confirm the remaining implementation details. | required |
| Scope is bounded | The plan's scope statement, files-to-touch list, and stated exclusions | Pass if the proposed work is one bounded change that addresses the reproduced issue without expanding into unrelated behavior or adjacent improvements. | required |
| Approach targets the cause | The plan's implementation approach read against its diagnosis and the repro evidence | Pass if the planned change addresses the likely cause supported by the evidence rather than only masking the visible symptom. If the cause is still uncertain, the approach must include a way to confirm it before making an irreversible change. | required |
| Plan is executable | The plan's files-to-touch list, implementation steps, repo-facts block, and relevant repository context | Pass if a contributor unfamiliar with the issue could identify the relevant code area, what behavior to change, and how to begin the work. Exact functions or final implementation details may remain to be confirmed when the plan names a concrete tracing or investigation step to resolve them. | required |
| Test plan proves the change | The plan's test plan read against the unit 2 repro steps and expected behavior | Pass if the tests rerun the reproduced failing behavior through the real code and state an observable expected result after the change. The plan must also preserve or check relevant existing behavior when the change could affect it. | required |
| Risks and unknowns are honest | The plan's risks, unknowns, assumptions, limitations, and claims of certainty read against the available evidence | Pass unless the plan presents a material uncertainty or limitation known from the package as settled fact, or makes a confidence claim contradicted by the evidence. Explicitly stated risks, testing limits, deferred questions, and implementation details that will be confirmed during the build are acceptable and do not fail this check. | required |
| Comment fits thread and repo | The draft plan comment read against the issue thread highlights, repo-facts block, contribution guidance, and applicable repository conventions | Pass if the comment accurately reflects the plan, is specific to the issue, respects relevant thread context, and follows requirements that explicitly apply to issue comments or contributor communication. If the repository explicitly requires AI-use disclosure in comments, the required disclosure must be present. General PR or code rules do not by themselves create a comment requirement. | required |

## Verdict rule

Accept only if every required check passes. Any failed or unclear required check makes the verdict reject. There are no preferred checks in this rubric.
