# Copy-Paste Setup for Consumers

This is a quick copy-paste guide for adding the security scan workflow to your repository. Follow these steps:

## 1️⃣ Copy Workflow File

Create `.github/workflows/security.yml` in your repository and paste:

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

## 2️⃣ Copy Dependabot Configuration

Create `.github/dependabot.yml` in your repository and paste:

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
      - "your-username"
    labels:
      - "dependencies"
      - "github-actions"
    commit-message:
      prefix: "chore(deps): "
      include: "scope"
```

**Important:** Change `"your-username"` to your actual GitHub username or team name.

## 3️⃣ Commit and Push

```bash
git add .github/
git commit -m "chore: add security scanning with shared-security-scan workflow"
git push origin main
```

## 4️⃣ Verify Setup

1. Go to your repository on GitHub
2. Click on **Actions** tab
3. You should see "Security Scan" workflow listed
4. Create a new pull request to trigger the workflow

## Done

The workflow will now:

- Run on every push to `main` and `develop`
- Run on every pull request
- Run daily at 2 AM UTC
- Automatically update when we release new versions (via Dependabot)

## What Dependabot Does

Every Monday at 8 AM UTC, Dependabot will:

1. Check for new versions of the `shared-security-scan` workflow
2. Create a pull request with the update
3. Add `dependencies` and `github-actions` labels
4. Ask you to review and merge

This ensures you always have the latest security checks!

## Customization

### Change the Schedule

In `.github/workflows/security.yml`, modify the `cron` schedule:

```yaml
schedule:
  - cron: '0 0 * * *'  # Daily at midnight UTC
```

### Scan Specific Directory

Change `scan-target` in the workflow:

```yaml
with:
  scan-target: './src'  # Only scan src folder
```

### Fail on Warnings

To make the workflow fail if issues are detected:

```yaml
jobs:
  security-scan:
    uses: renan-org/shared-security-scan/.github/workflows/security-scan.yml@v1.0.0
    with:
      scan-target: '.'

  check-results:
    needs: security-scan
    runs-on: ubuntu-latest
    steps:
      - name: Fail if warnings
        if: needs.security-scan.outputs.scan-result == 'warning'
        run: exit 1
```

## Troubleshooting

### Workflow not found error

```text
Error: The requested workflow was not found
```

**Fix:** Make sure:

- The repository path is `renan-org/shared-security-scan`
- The version tag exists (currently `@v1.0.0`)
- Your GitHub organization has access to the repository

### Dependabot PRs not created

1. Go to your repository **Settings**
2. Click **Code security and analysis**
3. Enable "Dependabot version updates"
4. Make sure `.github/dependabot.yml` exists

### Permission denied

If Dependabot can't commit:

1. Go to **Settings > Actions > General**
2. Under "Workflow permissions" select "Read and write permissions"
3. Click Save

## Next Steps

- Review the [CONSUMER_SETUP.md](CONSUMER_SETUP.md) for more advanced configuration
- Check the [examples/](examples/) folder for different workflow patterns
- See the main [README.md](README.md) for workflow details

---

**Need help?** Open an issue at [renan-org/shared-security-scan](https://github.com/renan-org/shared-security-scan)
