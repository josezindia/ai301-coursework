# Unit 4 — Test and Submit

## Your pull request

**Pull request**

https://github.com/josezindia/pathreview-ai301-fa26-s3/pull/1

**Branch**

fix/57-vendored-path-filtering

## Eval iterations

**Run history**

18/20

**Package analysis**

`pkg-02`

My rubric verdict: `reject`  
Gold label: `accept`

The eval output said: `"failed: test-evidence, diff-reviewability"`.

My rubric was too strict on this clear-accept package. It required more explicit test evidence than the package needed to demonstrate readiness, and the diff-reviewability check also treated acceptable implementation detail as a reason to hold the PR. The other clear-accept packages mostly passed, so this showed that the rubric could identify real failures but was slightly over-conservative on some valid PRs.

**Check rationale**

> | commit-hygiene | The commit list. | Pass when each commit message says what that commit changes ("cli: cap --threads at 4096"). Fail on messages like "wip", "fix", "misc", or "hopefully". | preferred |

I kept commit hygiene as `preferred` rather than `required` because a weak commit message can make review less convenient without making an otherwise correct, tested, scoped PR unsafe to submit. This lets the tool report the problem without rejecting a technically ready PR.

**Trade-offs**

Making `commit-hygiene` preferred means the tool can accept a PR whose commit message does not fully follow repository conventions, as long as the required checks pass. I accepted that trade-off because plan fidelity, test evidence, diff quality, description fidelity, and repository standards are stronger indicators of whether the PR itself is ready. The final full run still matched every failure category and reached 18/20.
