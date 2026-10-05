# Evidence guide

## Environment

Where it lives: In eval mode, use the repro report's environment record and the issue context or repo-facts block. In live mode, use the student's draft repro comment together with the issue and the repository's setup documentation.

What good looks like: The report names the operating system or platform, relevant runtime or tool versions, and any dependencies or configuration needed to reproduce the issue. The environment should match the issue's intended target, or any meaningful difference should be stated clearly.

## Steps

Where it lives: In eval mode, use the reproduction steps in the repro report and compare them with the issue context. In live mode, use the student's draft repro comment and the repository's setup or usage documentation when needed.

What good looks like: The steps begin from a clear starting state and include the actions needed to trigger the reported behavior. A stranger following them should be able to reach the same test, command, input, or workflow without having to guess an important missing step.

## Behavior shown

Where it lives: In eval mode, use the repro report's output excerpts, logs, screenshots, or other artifacts and compare them directly with the behavior described in the issue context. In live mode, use the student's draft repro comment and any attached or quoted evidence.

What good looks like: The evidence shows the same behavior the issue describes, not just a nearby failure or a different bug. The observed output should be specific enough that another person can compare it with the expected behavior and tell whether the issue was actually reproduced.

## Honesty

Where it lives: In eval mode, compare the repro report's stated outcome with the artifacts, output excerpts, logs, or screenshots included in the package. In live mode, compare the student's draft claim or repro comment with the evidence they provide.

What good looks like: The report states only what the evidence supports. A successful reproduction says the issue behavior was observed when the artifacts show it. A cannot-reproduce result also passes when the report clearly says it could not reproduce the issue and records what actually happened instead of claiming success.

## Comms

Where it lives: In eval mode, use the claim comment, repro comment, repo-facts block, contribution policy, issue context, and any repository templates. In live mode, use the student's draft comment together with the issue thread and the repository's contribution documentation.

What good looks like: The claim names the specific issue and accurately states what the contributor plans to investigate without promising a fix or a deadline. The repro comment is specific to the issue, reports the observed result honestly, and follows any repository communication requirements, including required AI-use disclosure or templates. Boilerplate that could apply to any issue does not pass.
