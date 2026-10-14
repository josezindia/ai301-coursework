# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

Where it lives: In eval mode, read the candidate plan's diagnosis together with the repro-evidence block and issue context. In live mode, read `plan.md` against the student's posted reproduction comment and the issue thread.

What good looks like: The stated cause or hypothesis explains behavior the reproduction actually demonstrated and does not contradict the recorded output. If the evidence supports only a likely cause rather than a proven root cause, the plan states that uncertainty explicitly.

## Scope

Where it lives: In eval mode, use the candidate plan's scope statement, files-to-touch list, exclusions, and implementation approach. In live mode, use the corresponding parts of `plan.md` and repository context when needed.

What good looks like: The plan describes one bounded change tied to the reproduced issue. It excludes unrelated cleanup, refactors, adjacent bugs, or behavior changes that are not needed to address the issue.

## Executability

Where it lives: In eval mode, use the candidate plan's files-to-touch list, implementation approach, work sequence, and repo-facts block. In live mode, use `plan.md` together with the relevant repository files and documentation.

What good looks like: Another contributor can identify where to start, what code or behavior to change, and the main sequence of work without inventing a missing technical decision.

## Test plan

Where it lives: In eval mode, read the candidate plan's test plan against the repro-evidence block, especially the original commands, inputs, expected result, and observed failure. In live mode, compare the test plan in `plan.md` with the student's Unit 2 reproduction steps and relevant existing tests.

What good looks like: The plan reruns the behavior that failed before through the real code and states an observable expected result after the fix. When the change could affect existing behavior, it also names an appropriate regression or existing-test check.

## Honesty

Where it lives: In eval mode, use the plan's risks, unknowns, assumptions, diagnosis language, and deviations. In live mode, use those parts of `plan.md` and compare them with the reproduction and repository evidence.

What good looks like: The plan distinguishes confirmed facts from hypotheses and unknowns. Material uncertainty that could affect the implementation is identified and, where practical, paired with a way to investigate it.

## Comms

Where it lives: In eval mode, read the candidate plan comment against the issue context, thread highlights, repo-facts block, contribution policy, and stated communication requirements. In live mode, read `comment.md` against the actual GitHub issue thread and repository contribution guidance.

What good looks like: The comment accurately reflects the plan, is specific to the issue, respects relevant thread context, and follows requirements that explicitly apply to issue comments or contributor communication.
