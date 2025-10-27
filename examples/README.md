# Consumer Examples

This folder contains example configurations that you can copy to your repository to use the `shared-security-scan` reusable workflow with automatic updates via Dependabot.

## Files in This Directory

### Workflow Examples

#### `security.yml`

Basic workflow that calls the reusable security scan on push, pull request, and on a daily schedule.

**Usage:**

1. Copy to `.github/workflows/security.yml` in your repository
2. Update the version tag if a newer version is available
3. Customize `scan-target` and `upload-artifact` as needed

#### `security-with-outputs.yml`

Advanced workflow that demonstrates:

- Calling the reusable workflow
- Using the output values (`scan-result`, `scan-timestamp`)
- Creating a downstream job that handles the scan results
- Failing the workflow if security issues are detected

**Usage:**

1. Copy to `.github/workflows/security.yml` in your repository
2. Customize the `handle-results` job logic as needed
3. Adjust branch triggers and schedule

### Dependabot Configuration Examples

#### `dependabot-basic.yml`

Simple Dependabot configuration for keeping GitHub Actions (including reusable workflows) up to date.

**Features:**

- Weekly updates on Mondays
- Creates PRs automatically
- Adds `dependencies` and `github-actions` labels

**Usage:**

1. Copy the content to `.github/dependabot.yml` in your repository
2. Update `reviewers` with your GitHub username/team
3. Commit and enable Dependabot if not already enabled

#### `dependabot-advanced.yml`

Advanced Dependabot configuration with auto-merge, multiple package ecosystems, and fine-tuned settings.

**Features:**

- Weekly updates for GitHub Actions on Mondays
- Auto-merge enabled for minor and patch updates
- Additional npm/pip dependency checking
- Assignees and reviewers
- Custom branch naming

**Usage:**

1. Copy the content to `.github/dependabot.yml` in your repository
2. Update reviewer/assignee settings
3. Adjust package ecosystems based on your project
4. Optional: Enable auto-merge in repository settings

## Quick Start

### Step 1: Choose Your Workflow

For most projects, start with `security.yml`:

```bash
mkdir -p .github/workflows
cp examples/security.yml .github/workflows/
```

For advanced use cases with output handling, use `security-with-outputs.yml`:

```bash
mkdir -p .github/workflows
cp examples/security-with-outputs.yml .github/workflows/security.yml
```

### Step 2: Choose Your Dependabot Config

For basic setup, use `dependabot-basic.yml`:

```bash
mkdir -p .github
cp examples/dependabot-basic.yml .github/dependabot.yml
```

For advanced features (auto-merge, multiple ecosystems), use `dependabot-advanced.yml`:

```bash
mkdir -p .github
cp examples/dependabot-advanced.yml .github/dependabot.yml
```

### Step 3: Customize

Edit the files you just copied:

- Update usernames/teams in Dependabot config
- Adjust triggers and schedule in workflow
- Modify `scan-target` if needed

### Step 4: Commit and Push

```bash
git add .github/
git commit -m "chore: add security scanning with shared-security-scan workflow"
git push origin main
```

## Important Notes

### Version Pinning

Always pin to a specific version tag:

```yaml
uses: renan-org/shared-security-scan/.github/workflows/security-scan.yml@v1.0.0
```

Never use:

- `@main` - unpredictable
- `@latest` - not supported

### Dependabot Permissions

Ensure your repository has the correct permissions for Dependabot:

1. Go to Settings > Actions > General
2. Set "Workflow permissions" to "Read and write permissions"
3. Or use a PAT (Personal Access Token) with `workflow` scope

### Branch Protection

If you have branch protection rules:

- Allow Dependabot to bypass requirements (optional)
- Or create a branch protection rule for Dependabot branches

## Troubleshooting

### Workflow Not Found

- Verify the repository path: `renan-org/shared-security-scan`
- Check that the version tag exists in the shared-security-scan repository
- Ensure the version is in the format `@vX.Y.Z`

### Dependabot Not Creating PRs

- Check if Dependabot is enabled: Settings > Code security and analysis
- Verify `.github/dependabot.yml` is properly formatted
- Check GitHub Actions are enabled in the repository

### Permissions Error

- Go to Settings > Actions > General
- Ensure "Workflow permissions" is set to "Read and write permissions"

## Next Steps

1. **Monitor Dependabot PRs** - Review and merge security scan workflow updates
2. **Review Scan Results** - Check artifacts uploaded by the workflow
3. **Configure Alerts** - Set up branch protection rules if needed
4. **Share with Team** - Copy this guide to your team's documentation

## Support

For questions about these examples:

- See [CONSUMER_SETUP.md](../CONSUMER_SETUP.md) for detailed documentation
- Check the [shared-security-scan](https://github.com/renan-org/shared-security-scan) repository
- Review [GitHub Dependabot documentation](https://docs.github.com/en/code-security/dependabot)
