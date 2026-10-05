# Unit 2 — Reproduction

## Issue

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/57

## Claim comment

Link:
https://github.com/codepath/pathreview-ai301-fa26-s3/issues/57#issuecomment-5987110208

Text:

> Hi, I'd like to investigate this issue #57 as a first contribution. I plan to reproduce the language-detection behavior where files under node_modules/ and build/ can influence the detected primary language, then trace how those paths are filtered in tech_detector.py. I'll report back with what I find before making any code changes.

### Claim reflection

I kept the claim specific to issue #57 and promised investigation rather than a fix. The live repro-check grader accepted it and confirmed that it followed the repository communication rules and my voice guide.

## Reproduction comment

Link:
https://github.com/codepath/pathreview-ai301-fa26-s3/issues/57#issuecomment-5987531321

Text:

> I reproduced issue #57 on my fork.
>
> Environment:
> - macOS 26.5.2
> - Python 3.11.5
> - pytest 9.1.1
> - commit 2f4e82f
>
> Steps:
> 1. Installed the project and dev dependencies with:
>    `python -m pip install -e ".[dev]"`
> 2. Ran the two issue-specific tests without honoring their xfail markers:
>    `pytest -q --runxfail tests/unit/test_tech_detector.py::TestTechDetector::test_node_modules_excluded tests/unit/test_tech_detector.py::TestTechDetector::test_build_directory_excluded`
>
> Observed:
> - `test_node_modules_excluded` expected `primary_language == "Python"` but got `"JavaScript"`.
> - `test_build_directory_excluded` expected `primary_language == "Python"` but got `"JavaScript"`.
>
> The captured log in both cases reported:
> `primary_lang=JavaScript`
>
> With `node_modules/` and `build/` paths present, the detector reports JavaScript instead of Python.
>
> I reproduced the issue using the two issue-specific unit tests. I did not separately run the issue's eight-file example.
>
> Result: reproduced successfully on my environment.

### Reproduction reflection

The two issue-specific tests are normally marked xfail, so the normal test run reports them as expected failures. Running them with `--runxfail` exposed the actual assertions and showed the same wrong primary-language behavior described in the issue. I also recorded the environment and commit so someone else can rerun the same test.

## Run history

- First full eval: 18/20 overall, but the disclosure category floor was not met.
- Targeted rerun of pkg-12 and pkg-20: 2/2.
- Next full run: 17/20.
- Inspected pkg-05, pkg-12, and pkg-20 and refined the reproduction-steps, honesty, and repository-communication rules.
- Targeted rerun of pkg-05, pkg-12, pkg-13, and pkg-20: 4/4.
- Final full saved run: 19/20.
- Final category results:
  - clear-accept: 7/8
  - disclosure: 1/1
  - no-evidence: 4/4
  - unfollowable-comms: 3/3
  - wrong-target: 4/4

## Package analysis

### pkg-20

Gold label: reject  
Tool verdict after refinement: reject

The package's repository explicitly required AI-use disclosure for issue comments. The candidate claim and repro comments did not include that disclosure. My original communication rule was too general, so I refined it to distinguish between general code or PR guidance and requirements that explicitly apply to issue comments. After that change, pkg-20 was correctly rejected while packages without comment-specific disclosure requirements could still pass.

## Check rationale

### Environment recorded

A reproduction needs enough environment information to interpret whether the result was produced under a relevant setup. I check the operating system or platform, runtime/tool versions, and meaningful configuration differences.

### Reproduction steps

The central behavior must be rerunnable without requiring someone to invent an important missing action. Supporting setup can be summarized when the triggering condition is still clear enough to recreate.

### Behavior matches issue

A failing command alone is not enough. The evidence must show the same behavior described by the issue rather than a nearby or unrelated failure.

### Honest outcome

The stated conclusion must match the evidence. A successful reproduction passes when the artifacts show the issue, and a cannot-reproduce result can also pass when it is stated honestly and supported by the recorded output.

### Repository communication

The claim must be specific to the issue and promise investigation rather than a fix or deadline. The repro comment must report the observed result honestly and follow any communication rules that explicitly apply, including AI-use disclosure when required for comments.

## Trade-offs

I made all five checks required because each proof family can prevent a bad reproduction package from being posted. This is stricter than using preferred checks, but it matches the assignment's focus on environment, followable steps, correct behavior, honest outcomes, and repository conventions.

I also avoided requiring a fixed number of steps, headings, or a particular report shape. The rubric judges whether the reproduction is actually usable and supported by evidence rather than whether it matches a template.
