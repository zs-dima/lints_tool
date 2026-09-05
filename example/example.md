# Using lints_tool

Include the rule set from `analysis_options.yaml`:

```yaml
include: package:lints_tool/lints_tool.yaml

formatter:
  page_width: 120
  trailing_commas: preserve
```

Depend on it as a dev dependency:

```yaml
dev_dependencies:
  lints_tool:
    git:
      url: https://github.com/zs-dima/lints_tool.git
      ref: v1.0.1
```

Then run the analyzer with infos and warnings fatal, which is what CI does:

```sh
dart analyze --fatal-infos --fatal-warnings
```

The DCM rules in the same file apply when a DCM licence is present (`dcm analyze .`); without one
they do nothing and the analyzer half still holds.
