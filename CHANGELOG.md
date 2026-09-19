# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.0] - 2026-09-19

Every rule below was measured on BreakerSonar and DoctorNoise before it was kept or dropped.

### Fixed

- `metrics:` was a YAML list of scalars where DCM expects a map, so 21 of its 22 thresholds had
  never applied. `dcm init metrics-preview --only-enabled` reported 1 metric; it now reports 15.
- `maintainability-index` carried a pre-1.35 value on what is now a 0..1 scale.
- `strict-raw-types: true` did nothing: `strict_raw_type` is the only diagnostic it emits, and
  `errors:` silenced it. That and three sibling suppressions cost 6 findings to remove.
- `unsafe_html` is removed from the Dart linter; `prefer_relative_imports` and
  `always_put_control_body_on_new_line` carried a severity while disabled.
- `enable-experiment` listed `macros` (cancelled 2025-01), `variance` and `const-functions`.
- `dcm: formatter: cascading-widget-extensions` is a removed flag, and `line-length` loses to
  Dart's `page_width`.
- `avoid-missing-image-alt` had been dropped as unknown; it was renamed to
  `provide-image-semantic-label` in 1.39 and is not web-only. Its whole family came back with it.

### Added

- `formatter:` ships with the include, so consumers stop repeating it.
- `extends: metrics-recommended` — a built-in preset, so the package still has no dependencies.
- 63 DCM rules: accessibility (13), security (4), async correctness (7), wrong-code catchers and
  four pubspec rules. 303 recognised rules became 366.
- 34 Dart lints, including the 8 the set had drifted behind `flutter_lints` 6.0.0, and
  `discarded_futures` / `no_dynamic_casts` / `no_raw_types`, first withdrawn for an overlap
  that re-measurement disproved (36, 1 and 0 findings).
- `curly_braces_in_flow_control_structures` back on: errorProne, part of that same baseline, and
  5 findings in BreakerSonar. It only asks for braces once the body leaves the `if` line.

### Measured, then scoped rather than dropped

A rule that earns its place stays on; the noise is handled by scoping it to where its premise
holds, not by deleting it.

- `avoid-unsafe-collection-methods`, `no-empty-block` and `avoid-non-null-assertion` are excluded
  from tests: there, `.first` throwing or a `!` blowing up IS the assertion. Numeric code that
  indexes by construction (a DSP package) excludes itself locally.
- `avoid-collection-mutating-methods` takes `ignore-private: true` — without it every
  `_field.add(x)` on an owned collection is a hit (112 of 125).
- `no-equal-arguments` ignores the layout parameters that are equal on purpose.
- `avoid-unused-parameters` takes `ignore-inline-functions: true`: builder signatures force them.

### Not enabled, with the reason recorded

- `avoid-future-ignore`: 102 of 103 hits are a deliberate, commented `.ignore()` — the explicit
  way to mark a future as intentionally not awaited.
- `prefer-correct-throws`: needs `@Throws` from `dart-code-metrics-annotations`.
- `no-empty-string`: bans the `''` literal.
- `unnecessary_type_name_in_constructor`: rewrites all 315 constructors to the 3.13 spelling.
  Left as `false` with the reason, ready for whenever primary constructors are adopted.

### Changed

- Install recipe is pub.dev, not a git ref.
- Requires DCM 1.39.2 or newer, the build the set was measured on. A `dcm_global.yaml` floor was
  tried and dropped: the check is Teams+, and it did not fire on Pro against a deliberate
  `>=99.0.0`.

## [1.0.1] - 2026-09-04

### Added

- First released version: one rule set for every repository that includes it, so a rule changes
  once rather than in every copy.
