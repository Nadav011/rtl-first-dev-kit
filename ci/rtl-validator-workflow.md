# RTL Validator Workflow

This is the first practical workflow sketch for blocking RTL regressions in pull requests.

## Trigger

- `pull_request` to `main` or `master`
- `push` to protected branches if the repo wants hard enforcement after merge

## Minimal Workflow Shape

```yaml
name: RTL Validator

on:
  pull_request:
    branches: [main]

jobs:
  rtl-validate:
    runs-on: [self-hosted, linux, x64, pop-os]
    steps:
      - uses: actions/checkout@v4
      - name: Scan physical-direction utilities
        run: |
          ! rg -n "ml-|mr-|pl-|pr-|left-|right-|text-left|text-right" src app components
```

## Reporting Rule

- Fail the job on the first regression
- Print file and line references from `rg`
- Keep the message short enough that the developer can act without opening the full logs

## Future Hooks

- Optional autofix preview job that comments a suggested patch
- Optional Flutter mode that scans `EdgeInsets.only(left|right)` and `Alignment.*Left|Right`
- Optional allowlist file for legacy migrations that are still in progress
