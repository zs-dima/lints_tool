# lints_tool

[![CI](https://github.com/zs-dima/lints_tool/actions/workflows/ci.yml/badge.svg)](https://github.com/zs-dima/lints_tool/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/license-MIT-purple.svg)](LICENSE)

A strict, prod-ready analyzer configuration for Dart and Flutter projects. Built on `flutter_lints`
and [DCM](https://dcm.dev), and used unchanged by every app and package in this estate.

## Use

```yaml
# analysis_options.yaml
include: package:lints_tool/lints_tool.yaml
```

```yaml
# pubspec.yaml
dev_dependencies:
  lints_tool: ^1.1.0
```

`formatter:` comes with the include — `page_width: 120`, `trailing_commas: preserve` — so a
consumer does not repeat it.

DCM 1.39.2 or newer: the rule set names rules added in 1.36-1.39, and DCM ignores a rule it does
not know without saying so, so an older binary quietly runs a different set.

## What it is

Two files. `lib/lints_tool.yaml` is the entry point an `analysis_options.yaml` includes;
`lib/src/analysis_options.yaml` holds the rules themselves, both the analyzer's and DCM's, so a
CI run and an IDE see the same set.

The DCM rules are read whether or not a DCM licence is present; without one they do nothing and
the analyzer half still applies.

## The shared CI recipe

`.github/workflows/` also holds the gate every repository in the estate runs, as reusable
workflows: `dart-package.yml`, `flutter-package.yml` and `test-report.yml`. They live next to the
rules because a repository that includes one includes the other, and a shared workflow has to sit
in a public repository for a public caller to reach it. They are not part of the published package
(`.pubignore`).

A caller pins the floating major tag:

```yaml
jobs:
  ci:
    uses: zs-dima/lints_tool/.github/workflows/dart-package.yml@v1
```

Inputs: `sdk` and `working-directory`, `test: false` for a package with no tests,
`publish-check: false` while a package is not publishable; the Flutter recipe takes
`flutter-version`, `channel` and `example-directory`. Move `v1` when a change is backwards
compatible (`git tag -f v1 && git push -f origin v1`); cut `v2` when a caller has to change its
inputs.

## Changelog

[CHANGELOG.md](CHANGELOG.md)

## License

[MIT](LICENSE)
