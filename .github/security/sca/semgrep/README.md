# Semgrep Code Scanner Action

This GitHub Action runs Semgrep, a fast, open-source static analysis tool for finding bugs and enforcing code standards.

It scans the workspace using the specified Semgrep configuration and, if a pull request ID is provided, comments the findings directly on the pull request.

## Example usage

```yaml
steps:
  - name: Checkout repository
    uses: actions/checkout@v4

  - name: Run Semgrep Code Scanner
    uses: un-fao/fao-github-actions-library/security/sca/semgrep@v1.4.x
    with:
      path: '.'
      config: 'auto'
      pr_id: ${{ github.event.pull_request.number }}
      github_token: ${{ secrets.GITHUB_TOKEN }}
      fail_on_findings: 'true'
```

## Inputs

| Name               | Description                                                                 | Default | Required |
|--------------------|-----------------------------------------------------------------------------|---------|----------|
| `path`             | Directory to scan                                                           | `.`     | No       |
| `config`           | Semgrep configuration (e.g., `auto`, `p/default`, `p/security-audit`)       | `auto`  | No       |
| `pr_id`            | Pull request ID for commenting results                                      |         | No       |
| `github_token`     | GitHub token for authentication (required if `pr_id` is provided)           |         | No       |
| `fail_on_findings` | Whether to fail the action if findings are found                            | `true`  | No       |
| `path`             | Directory to scan                                                           | `.`     | No       |
