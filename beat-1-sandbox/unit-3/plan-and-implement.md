# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

## Posted upstream

**GitHub username**

josezindia

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/57#issuecomment-6162002086

I reproduced issue #57, then inspected `_should_skip_file()` and found that its existing skip-directory patterns include a leading slash, so repo-root-relative paths like `node_modules/...` and `build/...` do not match.

My plan is to keep the fix local to `agent/tools/tech_detector.py` and make every directory already present in `skip_patterns` work consistently at both the repo root and nested positions. That means this will also make top-level `vendor/`, `dist/`, `.git/`, `__pycache__/`, `.venv/`, and `venv/` behave consistently with their nested forms. I am not adding any new skip-directory names; if maintainers would prefer this fix limited only to `node_modules/` and `build/`, I can narrow it before implementation.

The matching will remain directory-boundary-aware so lookalikes such as `rebuild/...` are not excluded accidentally.

I’ll remove the `xfail` markers from the two issue-specific tests once they pass normally, keep coverage for nested skipped directories, and add boundary coverage showing lookalike directory names are not skipped.

The separate `primary_language` selection behavior remains out of scope for #57.

For verification, I’ll re-run the Unit 2 reproduction tests and the full `tests/unit/test_tech_detector.py` file.

---

## Your branch

**Branch**

`fix/57-vendored-path-filtering`

**Evidence**

Before — Unit 2 reproduction:

```text
pytest -q --runxfail \
  tests/unit/test_tech_detector.py::TestTechDetector::test_node_modules_excluded \
  tests/unit/test_tech_detector.py::TestTechDetector::test_build_directory_excluded
