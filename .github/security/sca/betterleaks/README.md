# Betterleaks Secret Scanner

## Overview

This composite GitHub Action scans code for potential secrets (API keys, passwords, tokens, etc.) using [Betterleaks](https://github.com/betterleaks/betterleaks). It can optionally comment the results on a pull request and fail the build if leaks are detected.

## Inputs

| Name              | Description                                      | Required | Default |
| ----------------- | ------------------------------------------------ | -------- | ------- |
| `path`            | Directory to scan                                | ❌ No     | `.`     |
| `config_path`     | Path to Betterleaks configuration file           | ❌ No     | -       |
| `baseline_path`   | Path to Betterleaks baseline file                | ❌ No     | -       |
| `pr_id`           | Pull request ID for commenting results           | ❌ No     | -       |
| `github_token`    | GitHub token for authentication (needed for PR) | ❌ No     | -       |
| `validation`      | Enable validation of findings against live APIs  | ❌ No     | `false` |
| `fail_on_leaks`   | Whether to fail the action if leaks are found    | ❌ No     | `true`  |
| `command`         | Betterleaks command to run (`git` or `dir`)      | ❌ No     | `dir`    |

## Workflow Steps

1. **Betterleaks scan**: Runs the scanner using Docker (ghcr.io/betterleaks/betterleaks).
2. **Format results**: Parses the scan output and prepares a summary.
3. **PR Comment**: If `pr_id` and `github_token` are provided, posts a summary table to the PR.
4. **Enforce failure**: If `fail_on_leaks` is set to `true`, the action fails if any leaks are detected.
5. **Cleanup**: Removes temporary report files.

## Example Usage

### Basic Scan

```yaml
jobs:
  secrets-scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: un-fao/fao-github-actions-library/security/sca/betterleaks/betterleaks@v1.4.x
```

### Scan with PR Comment

```yaml
jobs:
  secrets-scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: un-fao/fao-github-actions-library/security/sca/betterleaks/betterleaks@v1.4.x
        with:
          pr_id: ${{ github.event.pull_request.number }}
          github_token: ${{ secrets.GITHUB_TOKEN }}
```

### Scan excluding known findings (Baseline)

```yaml
jobs:
  secrets-scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: un-fao/fao-github-actions-library/security/sca/betterleaks/betterleaks@v1.4.x
        with:
          baseline_path: .betterleaks-baseline.json
```
