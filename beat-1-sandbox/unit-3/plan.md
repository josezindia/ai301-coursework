# Plan for issue #57

## Diagnosis

The reproduced failure is caused by `_should_skip_file()` only checking directory patterns that include a leading slash, such as `/node_modules/` and `/build/`.

The Unit 2 reproduction showed:

- `test_node_modules_excluded` expected `primary_language == "Python"` but got `"JavaScript"`.
- `test_build_directory_excluded` expected `primary_language == "Python"` but got `"JavaScript"`.
- Both failures occurred when repo-root-relative paths such as `node_modules/...` and `build/...` were present.
- The current implementation in `agent/tools/tech_detector.py` checks:
  `return any(pattern in filepath for pattern in skip_patterns)`

Because top-level relative paths do not begin with `/`, patterns like `/node_modules/` and `/build/` do not match those paths.

There is a separate behavior in `_detect_tech()` where the primary language is selected from `sorted(languages)[0]` rather than by frequency. That is not part of issue #57 and will not be changed here.

## Scope

In scope:

- Update the existing `_should_skip_file()` directory list so every directory already in `skip_patterns` works both as a top-level repo-relative path and as a nested path.
- This includes the existing entries for `node_modules`, `vendor`, `dist`, `build`, `.git`, `__pycache__`, `.venv`, and `venv`; no new skip-directory names are being added.
- Preserve directory-boundary behavior so lookalike names such as `rebuild/...` are not skipped accidentally.
- Update the existing issue #57 regression tests so they pass normally instead of remaining expected failures.

Not in scope:

- Changing how `primary_language` is chosen.
- Refactoring the broader technology-detection logic.
- Changing framework detection.
- Expanding this issue into unrelated path normalization behavior.

## Files to touch

- `agent/tools/tech_detector.py`
  - Update `_should_skip_file()` so skip directories match both top-level relative paths and nested paths.
- `tests/unit/test_tech_detector.py`
  - Remove the issue #57 `xfail` markers once the fix makes the two regression tests pass.
  - Keep or extend coverage so both top-level and nested skip-directory behavior remain verified.

## Approach

1. Modify `_should_skip_file()` so every directory already in `skip_patterns` can match:
   - a repo-root-relative path beginning with that directory component, such as `node_modules/package/index.js`
   - the existing nested form, such as `src/node_modules/package/index.js`
   - without matching lookalike components such as `rebuild/app.js`

2. Keep the change local to `_should_skip_file()` rather than changing `_detect_tech()` or the language-selection logic.

3. Remove the strict `xfail` markers from:
   - `test_node_modules_excluded`
   - `test_build_directory_excluded`

4. Add or preserve a regression check for nested skipped directories so the fix for top-level paths does not break the behavior already handled by the current substring patterns.

## Test plan

Before the fix, the Unit 2 reproduction command was:

`pytest -q --runxfail tests/unit/test_tech_detector.py::TestTechDetector::test_node_modules_excluded tests/unit/test_tech_detector.py::TestTechDetector::test_build_directory_excluded`

Observed before the fix:

- `test_node_modules_excluded` failed because the detector returned `"JavaScript"` instead of `"Python"`.
- `test_build_directory_excluded` failed for the same reason.

After the fix:

1. Re-run the two issue-specific tests without `--runxfail` after removing their `xfail` markers.
2. Expect both tests to pass and report Python as the primary language for those fixtures.
3. Run the full `tests/unit/test_tech_detector.py` test file to check for regressions.
4. Verify a nested skipped path such as `src/node_modules/package/index.js` or `src/build/bundle.js` is still ignored.
5. Add a boundary regression case such as `rebuild/app.js` and verify it is still considered normally rather than being skipped merely because its component contains the word `build`.

## Risks and unknowns

The main risk is making the skip matching too broad and accidentally excluding a filename that merely contains a skip-directory name as part of another path component. The implementation should therefore match directory boundaries rather than arbitrary substrings.

The exact smallest implementation can be decided while editing `_should_skip_file()`, but it should remain limited to path filtering for the existing skip-directory list.

## Deviations

No material deviations from the posted plan.

The implementation stayed within `agent/tools/tech_detector.py` and `tests/unit/test_tech_detector.py`. `_should_skip_file()` now matches directory components against the existing skip-directory list, which covers both top-level and nested paths without matching lookalike names such as `rebuild`.

The two issue #57 `xfail` markers were removed, and regression coverage was added for nested skipped directories and a lookalike directory boundary case.

Verification completed successfully:
- 4 targeted tests passed.
- Full `tests/unit/test_tech_detector.py` suite passed: 29 tests.
