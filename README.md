# FAO GitHub Security Workflows

A centralized repository of reusable GitHub Actions and workflows designed to enforce security standards across the FAO GitHub organization.

## 🚀 Overview

This repository provides standardized security scanning templates for secret detection and Static Application Security Testing (SAST). By using these centralized workflows, FAO projects can ensure consistent security auditing without duplicating configuration effort.

## 🛡️ Included Tools

### 1. Betterleaks (Secret Scanning)
Scan for accidentally committed secrets, API keys, and sensitive tokens.
- **Location**: `.github/security/sca/betterleaks/`
- **Underlying Tool**: [Betterleaks](https://github.com/betterleaks/betterleaks)

### 2. Semgrep (SAST Scanning)
Scan code for security vulnerabilities, logic errors, and code quality issues.
- **Location**: `.github/security/sca/semgrep/`
- **Underlying Tool**: [Semgrep](https://github.com/semgrep/semgrep)

## 🛠️ Usage

### Organization-Wide Security Scanning

The standard workflow for organization-level scanning is located at:
`.github/workflows/org-sca-scanning.yaml`

This workflow is designed to run on `pull_request` events and performs both secret and SAST scanning.

### Using Local Actions

To use these security actions directly in another workflow within this repository:

#### Secret Scanning (Betterleaks)
```yaml
- name: Run Betterleaks
  uses: ./.github/security/sca/betterleaks
  with:
    pr_id: ${{ github.event.pull_request.number }}
    github_token: ${{ secrets.GITHUB_TOKEN }}
```

#### SAST Scanning (Semgrep)
```yaml
- name: Run Semgrep
  uses: ./.github/security/sca/semgrep
  with:
    pr_id: ${{ github.event.pull_request.number }}
    github_token: ${{ secrets.GITHUB_TOKEN }}
    fail_threshold: ERROR
```

## 📝 Configuration Options

### Betterleaks Inputs
| Name | Description | Default |
| ---- | ----------- | ------- |
| `path` | Directory to scan | `.` |
| `pr_id` | Pull request ID for commenting results | (Optional) |
| `github_token` | GitHub token for commenting | (Optional) |
| `fail_on_leaks` | Whether to fail the workflow if leaks are found | `true` |

### Semgrep Inputs
| Name | Description | Default |
| ---- | ----------- | ------- |
| `path` | Directory to scan | `.` |
| `config` | Semgrep rules configuration | `auto` |
| `pr_id` | Pull request ID for commenting results | (Optional) |
| `github_token` | GitHub token for commenting | (Optional) |
| `fail_threshold` | Severity level to trigger failure (ERROR, WARNING, INFO) | `ERROR` |

## ⚖️ License
This project is maintained by the FAO DevOps and Security teams.