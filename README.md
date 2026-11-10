# 128x64 Organization Policies

This repository is the central home for 128x64's shared GitHub community health files, organization-wide policy documents, and reusable automation.

GitHub uses the supported default community health files in this repository for organization-owned repositories that do not provide their own versions.

## Organization

128x64 operates across three divisions:

- **x64.AI**
- **128x CLOUD**
- **128x PLAY**

Harbor CLI is the flagship product of 128x64; the organization also develops other products and projects.

## Divisions

See [DIVISIONS.md](DIVISIONS.md) for the division structure, current repository assignments, and organization-wide repositories.

## Shared policies

- [Code of Conduct](CODE_OF_CONDUCT.md)
- [Contributing Guidelines](CONTRIBUTING.md)
- [Governance](GOVERNANCE.md)
- [Security Policy](SECURITY.md)
- [Support](SUPPORT.md)
- [Pull Request Template](PULL_REQUEST_TEMPLATE.md)
- [Bug Report Template](.github/ISSUE_TEMPLATE/bug_report.md)
- [Feature Request Template](.github/ISSUE_TEMPLATE/feature_request.md)

A repository can still supply its own supported community health files and templates, which take precedence over these organization defaults.

## Licensing

The `licenses/` directory contains the source texts used by the license automation:

- [Public repository license](licenses/Public_LICENSE.md)
- [Internal repository license](licenses/Internal_LICENSE.md)

GitHub does not provide organization-wide default LICENSE files. Each project repository needs its own license file. The automation copies the public license to public repositories and the internal license to private repositories, but only when no existing license file is found. Existing license files are reported as conflicts and are never overwritten.

These license texts should receive qualified legal review before commercial reliance.

## Automation

- [Apply Organization License Policy](.github/workflows/default-license.yml) scans the organization every 15 minutes and can also be run manually. It requires the 128x64 License Bot GitHub App.
- [Validate Organization Defaults](.github/workflows/validate-defaults.yml) checks the central policy repository on pushes and pull requests.
- [Dependabot](.github/dependabot.yml) checks GitHub Actions dependencies weekly.

See [GitHub App setup](docs/GITHUB_APP_SETUP.md) to configure the license automation.

## Contact

For organization-wide questions, contact **128x64** at raphaelcvarghese@gmail.com.
