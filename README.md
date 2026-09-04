# lints_tool

[![CI](https://github.com/zs-dima/lints_tool/actions/workflows/ci.yml/badge.svg)](https://github.com/zs-dima/lints_tool/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/license-MIT-purple.svg)](LICENSE)

A strict, prod-ready analyzer configuration for Dart and Flutter projects. Built on `flutter_lints`
and [DCM](https://dcm.dev), and used unchanged by every app and package in this estate.

## Use

```yaml
# analysis_options.yaml
include: package:lints_tool/lints_tool.yaml

formatter:
  page_width: 120
  trailing_commas: preserve
```

```yaml
# pubspec.yaml
dev_dependencies:
  lints_tool:
    git:
      url: https://github.com/zs-dima/lints_tool.git
      ref: v1.0.1
```

A git dependency in `dev_dependencies` is invisible to anyone who depends on your package, so it
does not stop you publishing.

## What it is

Two files. `lib/lints_tool.yaml` is the entry point an `analysis_options.yaml` includes;
`lib/src/analysis_options.yaml` holds the rules themselves, both the analyzer's and DCM's, so a
CI run and an IDE see the same set.

The DCM rules are read whether or not a DCM licence is present; without one they do nothing and
the analyzer half still applies.

## Changelog

[CHANGELOG.md](CHANGELOG.md)

## License

[MIT](LICENSE)
