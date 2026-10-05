# Rubric: is this reproduction package ready to post?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Environment recorded | Repro report environment record, read against the issue context and repo-facts block | Pass if the report identifies the operating system or platform, relevant runtime or tool versions, and any dependency or configuration details needed to interpret the result. If the environment differs meaningfully from the issue's target, the difference must be stated. | required |
| Reproduction steps | Repro report reproduction steps, read against the issue context and repository setup documentation when needed | Pass if the report provides enough commands, actions, inputs, or workflow detail to rerun the central behavior being tested. Supporting setup or input may be summarized when the specific condition that triggers the issue is identified clearly enough to recreate it. | required |
| Behavior matches issue | Repro report artifacts such as output excerpts, logs, screenshots, or test results, read against the behavior described in the issue context | Pass if the evidence shows the same behavior described by the issue rather than a different or adjacent failure, and the observed result is specific enough to compare with the expected behavior. | required |
| Honest outcome | Repro report stated outcome compared with the included artifacts, output excerpts, logs, screenshots, or test results | Pass if the stated outcome agrees with the evidence shown. A successful reproduction passes when the artifacts show the reported issue behavior; a cannot-reproduce result passes when the artifacts support that result. Minor wording differences do not fail if the conclusion matches the evidence. | required |
| Repository communication | Claim comment and repro comment, read against the issue context, repo-facts block, contribution policy, and repository templates | Pass if the claim is specific to the issue and states the planned investigation without promising a fix or deadline, and the repro comment reports the observed result honestly. Follow requirements that explicitly apply to issue comments or contributor communication. If the repository explicitly requires AI-use disclosure for comments, the required disclosure must be present. General code or PR guidance does not by itself create a comment requirement. | required |

## Verdict rule

Accept if every required check passes. Reject if any required check fails. Treat unclear as fail for required checks. Preferred checks, if added later, do not change the verdict.
