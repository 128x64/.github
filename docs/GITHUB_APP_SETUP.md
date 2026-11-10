# Set up the 128x64 License Bot GitHub App

This guide configures the GitHub App used by the organization license workflow. The App needs repository metadata access and permission to read and write repository contents.

## 1. Create the App under the 128x64 organization

1. Sign in to GitHub with an account that can administer the 128x64 organization.
2. Open [128x64 organization settings](https://github.com/organizations/128x64/settings).
3. In the sidebar, find **Developer settings** → **GitHub Apps** → **New GitHub App**. If the owner selector appears, choose **128x64** as the App owner, not your personal account.
4. Set:
   - **GitHub App name:** `128x64 License Bot` (or another available unique name).
   - **Homepage URL:** `https://github.com/128x64`.
5. Under **Webhook**, turn **Active** off. This workflow polls every 15 minutes, so a webhook is not needed.
6. Under **Repository permissions**, set:
   - **Metadata:** Read-only (GitHub requires this).
   - **Contents:** Read and write.
7. Do not grant other repository or organization permissions. This workflow does not need administration, members, issues, or pull request access.
8. Under **Where can this GitHub App be installed?**, choose **Only on this account**.
9. Click **Create GitHub App**.

## 2. Generate a private key

1. On the App settings page, locate **Private keys**.
2. Click **Generate a private key**. GitHub downloads a PEM file.
3. Copy the App's **Client ID** from the App settings page. The Client ID is different from the numeric App ID.

Keep the PEM file private. Do not paste it into chat, commit it to Git, or put it in a workflow file.

## 3. Install the App on 128x64

1. In the App settings sidebar, select **Install App**.
2. Find **128x64** and click **Install**.
3. Select **All repositories** so repositories created later are included automatically.
4. Confirm installation.

Install this App only on the 128x64 organization.

## 4. Add the workflow configuration

Open [128x64/.github Actions variables](https://github.com/128x64/.github/settings/variables/actions):

1. Create a repository variable named `LICENSE_BOT_CLIENT_ID`.
2. Set its value to the App's **Client ID**.

Then open [128x64/.github Actions secrets](https://github.com/128x64/.github/settings/secrets/actions):

1. Create a repository secret named `LICENSE_BOT_PRIVATE_KEY`.
2. Set its value to the entire downloaded PEM file, including the BEGIN and END lines.

Do not add the private key as a variable; it must be a secret.

## 5. Run a manual test

1. Open [the license workflow](https://github.com/128x64/.github/actions/workflows/default-license.yml).
2. Select **Run workflow** and run it on `main`.
3. Open the run and inspect the **128x64 license policy scan** summary.

Expected behavior:

- Public repositories without a recognized existing license receive `LICENSE` copied from `licenses/Public_LICENSE.md`.
- Private repositories without a recognized existing license receive `LICENSE` copied from `licenses/Internal_LICENSE.md`.
- Existing root files matching `LICENSE*`, `LICENCE*`, `COPYING*`, or `UNLICENSE` are reported as conflicts and left unchanged.
- Forks, archived repositories, and the central `.github` repository are skipped.

## 6. Automatic operation

After setup, the workflow runs every 15 minutes and can also be started manually. New repositories are discovered on the next scan. GitHub notes that scheduled workflows can be delayed during periods of high load.

A repository whose branch rules block the App from writing will be reported as a failure. The workflow does not change branch protections or force-push.

## Troubleshooting

- **App token creation fails:** confirm the App is installed on 128x64 and has Metadata read plus Contents read/write.
- **Repositories are missing:** confirm the installation is set to **All repositories**.
- **Secret or variable is missing:** verify the exact names `LICENSE_BOT_CLIENT_ID` and `LICENSE_BOT_PRIVATE_KEY`.
- **Permission denied on a repository:** check App installation access and the repository's branch protection or rulesets.
- **No scheduled runs:** confirm Actions are enabled and the workflow is on the default branch, `main`.

The workflow never replaces an existing license file.
