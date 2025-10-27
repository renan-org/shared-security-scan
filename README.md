# shared-security-scan

A reusable GitHub Actions workflow for performing security scans across repositories. This repository provides a self-contained security scanning solution that other repositories can reference and invoke.

## Features

- 🔍 Secret scanning and detection
- 🛡️ Vulnerability pattern analysis
- 📊 Customizable scan targets
- 📤 Artifact upload for scan reports
- 📌 Version-based releases for reproducible scans
- 🔄 Reusable workflow pattern

## Usage

### Basic Usage

In your repository's workflow, reference this reusable workflow:

```yaml
name: My Security Check

on:
  push:
    branches:
      - main
  pull_request:

jobs:
  security:
    uses: renan-org/shared-security-scan/.github/workflows/security-scan.yml@v1.0.0
    with:
      scan-target: '.'
      upload-artifact: true
```

### Getting Started Quickly

For quick setup in your repository, see:

- **[CONSUMER_SETUP.md](CONSUMER_SETUP.md)** - Complete setup guide with Dependabot configuration
- **[examples/](examples/)** - Ready-to-copy workflow and Dependabot configurations

### Using Specific Versions

To ensure reproducibility, always reference a specific version tag:

```yaml
uses: renan-org/shared-security-scan/.github/workflows/security-scan.yml@v1.0.0
```

Get the latest released version from [Releases](../../releases).

### Workflow Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `scan-target` | No | `.` | Target directory or path to scan |
| `upload-artifact` | No | `true` | Upload scan results as GitHub artifact |

### Workflow Outputs

| Output | Description |
|--------|-------------|
| `scan-result` | Security scan result status (passed/warning) |
| `scan-timestamp` | Timestamp when scan was performed |

### Using Outputs in Your Workflow

```yaml
jobs:
  security:
    uses: renan-org/shared-security-scan/.github/workflows/security-scan.yml@v1.0.0
    with:
      scan-target: '.'

  downstream-job:
    needs: security
    runs-on: ubuntu-latest
    steps:
      - name: Check scan result
        run: |
          echo "Scan result: ${{ needs.security.outputs.scan-result }}"
          echo "Scan time: ${{ needs.security.outputs.scan-timestamp }}"
```

## Release Management

This repository uses semantic versioning. New versions are released by creating a git tag:

```bash
git tag v1.0.0
git push origin v1.0.0
```

The release workflow automatically:

1. Creates a GitHub Release
2. Generates a changelog
3. Archives the workflow files

**Consumer repositories should always pin a specific version tag** to ensure consistent, reproducible builds.

## Development

### Local Testing

To test the workflow locally, you can use [act](https://github.com/nektos/act):

```bash
act push -j security-scan
```

### Modifying the Workflow

Edit `.github/workflows/security-scan.yml` to customize the security checks performed.

## License

MIT License - see [LICENSE](LICENSE) file for details.

## Support

For issues or questions, please open an issue in this repository.
