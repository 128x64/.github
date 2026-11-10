# GitHub App Setup: 128x64 License Bot

This guide documents the GitHub App used by the organization-wide license automation in this repository.

## App configuration

Create or review the App under the 128x64 organization:

https://github.com/organizations/128x64/settings/apps

Recommended configuration for this workflow:

- **Name:** 128x64 License Bot
- **Homepage URL:** https://github.com/128x64
- **Webhook:** disabled; this workflow uses scheduled scans and manual dispatch.
- **Repository permissions:**
  - Metadata: read-only (required by GitHub).
  - Contents: read and write.
- **Organization permissions:** none.
- **Installation scope:** install on the 128x64 organization and grant access to all repositories so newly created repositories are included.

Do not grant unrelated permissions.

## Credentials

1. Copy the App's **Client ID**. The workflow expects the Client ID, not the numeric App ID.
2. Generate a private key from the App settings.
3. In this repository's Actions settings, add:
   - Repository variable `LICENSE_BOT_CLIENT_ID` containing the Client ID.
   - Repository secret `LICENSE_BOT_PRIVATE_KEY` containing the complete PEM private key.
4. Never commit the private key, paste it into issues or discussions, or include it in logs.

Settings pages:

- [Actions variables](https://github.com/128x64/.github/settings/variables/actions)
- [Actions secrets](https://github.com/128x64/.github/settings/secrets/actions)

## Run and verify

Open the [Apply Organization License Policy workflow](https://github.com/128x64/.github/actions/workflows/default-license.yml) and use **Run workflow**.

Review the run summary. The workflow:

- Skips the central `.github` repository and archived repositories.
- Selects the public license for public repositories and the internal license for private repositories.
- Does not overwrite an existing root license-like file; it reports a conflict instead.
- Scans periodically and can also be run manually.

The schedule is every 15 minutes, but GitHub Actions may delay scheduled runs. Newly created repositories are discovered on a later scan.

## Troubleshooting

- If the workflow reports that setup is incomplete, verify the variable and secret names exactly.
- If token creation fails, confirm the App is installed on the 128x64 organization and the private key is current.
- If a repository is skipped because of an existing license, review that license manually; the workflow intentionally does not replace it.
- If a write fails, review the target repository's default branch protection and the App's Contents permission.
- Empty repositories or repositories without a default branch may be skipped until initialized.

This guide describes the intended configuration. It does not store credentials or prove that the App is currently installed or configured.
