# Consumer Setup Guide

This guide explains how to use the `shared-security-scan` reusable workflow in your repository and set up automatic updates via Dependabot.

## Quick Start

### Step 1: Add the Workflow to Your Repository

Create a workflow file in your repository at `.github/workflows/security.yml`:

```yaml
name: Security Scan

on:
  push:
    branches:
      - main
      - develop
  pull_request:
  schedule:
    # Run daily at 2 AM UTC
    - cron: '0 2 * * *'

jobs:
  security-scan:
    uses: renan-org/shared-security-scan/.github/workflows/security-scan.yml@v1.0.0
    with:
      scan-target: '.'
      upload-artifact: true
```

### Step 2: Enable Dependabot for Workflow Updates

Create `.github/dependabot.yml` in your repository:

```yaml
version: 2

updates:
  # Workflow updates
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"
      day: "monday"
      time: "08:00"
    reviewers:
      - "your-username"
    labels:
      - "dependencies"
      - "github-actions"
    commit-message:
      prefix: "chore(deps): "
      include: "scope"
```

### Step 3: Configure Dependabot Permissions (if using branch protection)

If your repository has branch protection enabled, you may need to:

1. Create a GitHub personal access token with `workflow` scope
2. Add it as a repository secret named `DEPENDABOT_TOKEN`
3. Update your branch protection rules to allow Dependabot commits

## Workflow Features

### Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `scan-target` | No | `.` | Target directory or path to scan |
| `upload-artifact` | No | `true` | Upload scan results as GitHub artifact |

### Outputs

| Output | Description |
|--------|-------------|
| `scan-result` | Security scan result status (passed/warning) |
| `scan-timestamp` | Timestamp when scan was performed |

## Advanced Usage

### Using Outputs in Downstream Jobs

```yaml
jobs:
  security-scan:
    uses: renan-org/shared-security-scan/.github/workflows/security-scan.yml@v1.0.0
    with:
      scan-target: '.'

  report:
    needs: security-scan
    runs-on: ubuntu-latest
    steps:
      - name: Check scan results
        run: |
          echo "Scan Status: ${{ needs.security-scan.outputs.scan-result }}"
          echo "Scan Time: ${{ needs.security-scan.outputs.scan-timestamp }}"
          
      - name: Fail if security issues found
        if: needs.security-scan.outputs.scan-result == 'warning'
        run: exit 1
```

### Running Only on Pull Requests

```yaml
on:
  pull_request:

jobs:
  security-scan:
    uses: renan-org/shared-security-scan/.github/workflows/security-scan.yml@v1.0.0
```

### Running on Schedule (Daily)

```yaml
on:
  schedule:
    - cron: '0 2 * * *'  # Daily at 2 AM UTC

jobs:
  security-scan:
    uses: renan-org/shared-security-scan/.github/workflows/security-scan.yml@v1.0.0
```

## Dependabot Configuration Details

### Automatic Update Strategy

The Dependabot configuration will:

1. **Check for updates** weekly on Mondays
2. **Create pull requests** automatically when new versions are available
3. **Auto-merge** (optional) if CI passes
4. **Label** updates for easy filtering

### Extended Configuration with Auto-Merge

```yaml
version: 2

updates:
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"
      day: "monday"
      time: "08:00"
    reviewers:
      - "your-team"
    assignees:
      - "your-username"
    labels:
      - "dependencies"
      - "github-actions"
    commit-message:
      prefix: "chore(deps): "
      include: "scope"
    pull-request-branch-name:
      separator: "/"
    # Auto-merge patch and minor updates
    auto-merge:
      enabled: true
      rules:
        - update-types:
            - "minor"
            - "patch"
          automerge-strategy: "auto"
```

## Viewing Dependabot Updates

### In Your Repository

1. Go to **Pull Requests** tab
2. Filter by label `dependencies` to see Dependabot PRs
3. Review the changes and merge when ready

### Checking for Security Updates

Dependabot will show:

- 🟢 Green - No security issues
- 🟡 Yellow - Low risk
- 🔴 Red - Critical security update (high priority)

## Best Practices

✅ **DO:**

- Always pin to specific versions (e.g., `@v1.0.0`, not `@main`)
- Review Dependabot PRs before merging
- Enable branch protection to require CI checks
- Keep your `.github/dependabot.yml` configuration simple

❌ **DON'T:**

- Use `@main` or `@master` references (unpredictable)
- Disable Dependabot for GitHub Actions
- Ignore Dependabot alerts for security issues
- Forget to enable workflow permissions for Dependabot

## Troubleshooting

### Dependabot PRs Not Created

**Check:**

1. `.github/dependabot.yml` file exists
2. GitHub Actions are enabled in your repository
3. Dependabot is enabled (Settings > Code security and analysis)

### Workflow Reference Not Found

**Error:** `Error: The requested workflow was not found`

**Solution:**

- Verify the version tag exists: `renan-org/shared-security-scan` repository
- Check the tag is in the format `@vX.Y.Z`
- Ensure repository is public or you have access

### Dependabot Can't Commit

**Issue:** Dependabot PRs are not created but no errors shown

**Solution:**

1. Check Repository Settings > Actions > Workflow permissions
2. Enable "Read and write permissions"
3. Or create a fine-grained personal access token with `workflow` scope

## Support

For issues with the `shared-security-scan` workflow:

- Check the main repository: [renan-org/shared-security-scan](https://github.com/renan-org/shared-security-scan)
- Open an issue with the workflow name and version

For Dependabot issues:

- See [GitHub Dependabot documentation](https://docs.github.com/en/code-security/dependabot)
