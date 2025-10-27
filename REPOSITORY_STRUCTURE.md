# Repository Structure & Documentation Guide

This document describes the complete structure of the `shared-security-scan` reusable workflow repository.

## 📁 Repository Structure

```text
shared-security-scan/
├── .github/
│   └── workflows/
│       ├── security-scan.yml          # The reusable workflow
│       └── release.yml                 # Release management workflow
├── examples/
│   ├── README.md                       # Guide for example files
│   ├── security.yml                    # Basic workflow example
│   ├── security-with-outputs.yml       # Advanced workflow example
│   ├── dependabot-basic.yml            # Basic Dependabot config
│   └── dependabot-advanced.yml         # Advanced Dependabot config
├── .gitignore                          # Git ignore rules
├── action.yml                          # Workflow metadata
├── LICENSE                             # MIT License
├── README.md                           # Main documentation
├── QUICKSTART.md                       # Quick setup guide
├── CONSUMER_SETUP.md                   # Detailed consumer guide
└── REPOSITORY_STRUCTURE.md             # This file
```

## 📖 Documentation Files

### For Repository Users (Consumers)

**Start here if you want to use this workflow in your repository:**

1. **[QUICKSTART.md](QUICKSTART.md)** ⭐
   - Copy-paste setup in 4 steps
   - Best for: Getting started quickly
   - Time: 5 minutes

2. **[examples/README.md](examples/README.md)**
   - Explains each example file
   - Shows which to use for your use case
   - Best for: Choosing the right configuration

3. **[CONSUMER_SETUP.md](CONSUMER_SETUP.md)**
   - Complete setup guide
   - Dependabot configuration details
   - Troubleshooting section
   - Best for: Understanding all options

### For Repository Maintainers

**Read if you maintain this repository:**

1. **[README.md](README.md)**
   - Features and capabilities
   - Release management information
   - Development and testing info

## 🚀 Quick Navigation

### I want to...

**Use this workflow in my repository**  
→ Read [QUICKSTART.md](QUICKSTART.md)

**Understand all configuration options**  
→ Read [CONSUMER_SETUP.md](CONSUMER_SETUP.md)

**See example configurations**  
→ Browse [examples/](examples/)

**Keep my workflow updated automatically**  
→ Copy one of the [dependabot configs](examples/)

**Set up advanced workflows with outputs**  
→ Use [security-with-outputs.yml](examples/security-with-outputs.yml)

**Contribute or maintain this repo**  
→ Read [README.md](README.md)

## 📋 Example Files

### Workflow Examples

| File | Purpose | Use When |
|------|---------|----------|
| `security.yml` | Basic security scan | Just starting out |
| `security-with-outputs.yml` | Advanced with output handling | Need scan results in other jobs |

### Dependabot Examples

| File | Purpose | Use When |
|------|---------|----------|
| `dependabot-basic.yml` | Simple weekly updates | Basic needs |
| `dependabot-advanced.yml` | Auto-merge, multi-ecosystem | Production setup |

## 🔄 Workflow Reference

### Main Reusable Workflow

**File:** `.github/workflows/security-scan.yml`

**Inputs:**

- `scan-target` (optional, default: `.`) - Directory to scan
- `upload-artifact` (optional, default: `true`) - Upload results

**Outputs:**

- `scan-result` - Status (passed/warning)
- `scan-timestamp` - When scan was performed

**Triggers:**

- Can be called by any repository in the organization
- Can be used in scheduled workflows
- Can be used on push, pull_request, or manual triggers

### Release Workflow

**File:** `.github/workflows/release.yml`

**Triggers:** On git tag (`v*`)

**Actions:**

1. Creates GitHub Release
2. Generates changelog
3. Archives workflow files

## 🛠️ How to Use as a Consumer

### Minimal Setup (5 minutes)

```bash
# 1. Copy the workflow file
mkdir -p .github/workflows
curl https://raw.githubusercontent.com/renan-org/shared-security-scan/main/examples/security.yml \
  -o .github/workflows/security.yml

# 2. Copy Dependabot config
mkdir -p .github
curl https://raw.githubusercontent.com/renan-org/shared-security-scan/main/examples/dependabot-basic.yml \
  -o .github/dependabot.yml

# 3. Edit dependabot.yml and change "your-username"
# 4. Commit and push
git add .github/
git commit -m "chore: add security scanning"
git push origin main
```

### Full Setup with All Options (15 minutes)

1. Read [CONSUMER_SETUP.md](CONSUMER_SETUP.md)
2. Choose your configuration from [examples/](examples/)
3. Copy relevant files to your repository
4. Customize as needed

## 📌 Versioning

This repository uses **semantic versioning**:

- **v1.0.0** - Current stable version
- **v1.x.x** - Minor/patch updates (safe to auto-merge)
- **v2.x.x** - Major updates (breaking changes)

**Always pin to a specific version:**
```yaml
uses: renan-org/shared-security-scan/.github/workflows/security-scan.yml@v1.0.0
```

Never use `@main` or `@master`!

## 🔐 Security & Permissions

### Required Permissions

To use Dependabot with this workflow:

1. **GitHub Actions Permissions**
   - Settings > Actions > General
   - "Workflow permissions" = "Read and write permissions"

2. **Optional: Dependabot Permissions**
   - Create fine-grained PAT with `workflow` scope
   - Set as repository secret `DEPENDABOT_TOKEN`

### Best Practices

- ✅ Always use specific version tags
- ✅ Enable branch protection rules
- ✅ Review Dependabot PRs before merging
- ✅ Keep Dependabot enabled
- ❌ Don't use `@main` references
- ❌ Don't ignore Dependabot security alerts

## 📞 Support & Issues

**For issues with this workflow:**

- Check [CONSUMER_SETUP.md](CONSUMER_SETUP.md) troubleshooting section
- Open an issue on GitHub
- Review example configurations

**For GitHub Actions/Dependabot questions:**

- See [GitHub Actions Documentation](https://docs.github.com/en/actions)
- See [GitHub Dependabot Documentation](https://docs.github.com/en/code-security/dependabot)

## 🔄 Update Process

### Getting Updates

Dependabot automatically creates PRs when new versions are released:

1. Monitor your repository's Pull Requests
2. Filter by label `github-actions`
3. Review and merge Dependabot PRs

### Staying Current

- Weekly Dependabot checks (default)
- Auto-merge available (advanced setup)
- Always pinned to explicit versions

## 📚 Additional Resources

- [Main README](README.md) - Feature overview
- [QUICKSTART](QUICKSTART.md) - 5-minute setup
- [CONSUMER_SETUP](CONSUMER_SETUP.md) - Complete guide
- [Examples](examples/) - Ready-to-use configs

---

**Last Updated:** October 2025
**Version:** 1.0.0
**License:** MIT
