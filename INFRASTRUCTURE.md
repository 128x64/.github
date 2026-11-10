# 128x64 Infrastructure Standards

This document defines the baseline infrastructure practices for repositories owned by the 128x64 GitHub organization. It is a shared operational standard; repository-specific workflows may add stricter controls where appropriate.

## 1. Repository ownership and access

- Keep project repositories owned by the 128x64 organization.
- Use the existing division teams for division-owned repositories and organization-wide teams/access for shared repositories.
- Grant the minimum repository role needed for each person's responsibilities.
- Review membership and repository access when responsibilities change or someone leaves a project.
- Do not use shared personal accounts or share credentials.

Current repository assignments are recorded in [DIVISIONS.md](DIVISIONS.md).

## 2. Default-branch protection

For active repositories with collaborative development, configure rules for the default branch:

- Require changes to be submitted through Pull Requests.
- Require at least one approval from an authorized reviewer for production or important shared repositories.
- Dismiss stale approvals when new commits materially change the reviewed code.
- Require approval of the most recent reviewable push where supported and appropriate.
- Require status checks only after the repository has stable CI checks configured.
- Block force pushes and branch deletion.
- Limit bypass permissions to the smallest necessary administrator group.

Small or experimental repositories may use a lighter review requirement, but should still avoid accidental force pushes and deletion.

## 3. GitHub Actions security

- Set workflow-level permissions to read-only by default.
- Grant write permissions only to the job and token that need them.
- Set `persist-credentials: false` on checkout steps unless later steps genuinely need authenticated Git operations.
- Store credentials in GitHub Actions secrets or environments; never commit them to source control or print them in logs.
- Prefer maintained actions and review action updates. For high-assurance workflows, pin third-party actions to full commit SHAs.
- Avoid privileged workflows that execute untrusted pull-request code with write-capable credentials.
- Use protected GitHub Environments and required reviewers for production deployments when deployments are introduced.

## 4. Dependency and code security

Where supported by the repository's GitHub plan and language ecosystem:

- Enable Dependabot alerts and security updates.
- Keep dependency update automation enabled for supported package managers and GitHub Actions.
- Enable secret scanning and push protection where available.
- Add code scanning or static analysis when the repository's language and build system are established.
- Review security alerts and assign an owner for remediation.

Do not enable checks that cannot be maintained or that provide misleading coverage.

## 5. Backups and recovery

GitHub is the collaboration host, not the only backup location.

- Maintain periodic backups of important repositories in a separate, access-controlled storage location.
- Preserve Git history, branches, tags, and other required refs; a working-directory ZIP is not a complete Git backup.
- Separately plan exports for issues, pull requests, releases, Actions configuration, and Discussions where those records matter. A Git mirror alone does not include all GitHub collaboration data.
- Encrypt backup storage and restrict access to designated administrators.
- Document backup frequency, retention, storage location, and restoration owner outside public repositories.
- Test restoration periodically and record the result.

Never place backup credentials or private storage locations containing sensitive access details in this public policy repository.

## 6. Releases and artifacts

When a repository begins publishing releases:

- Use a consistent versioning scheme appropriate to the project.
- Create releases from reviewed commits on the intended release branch.
- Include concise release notes, compatibility information, and known limitations.
- Keep generated artifacts associated with the corresponding release and source revision.
- Do not publish secrets, private datasets, debug files, or internal configuration in release assets.
- Document any signing, provenance, or artifact verification process that is actually implemented.

## 7. Operational ownership

Each active repository should identify who is responsible for:

- CI failures and dependency alerts.
- Access reviews.
- Release approval.
- Backup and recovery checks, where applicable.
- Security reports and remediation.

Responsibilities should be assigned to existing maintainers or division teams; this document does not create new teams or grant permissions.

## 8. Implementation status

This document describes the baseline to implement and periodically review. It does not claim that every listed setting is already enabled. Repository settings that cannot be verified through the available GitHub integration must be checked in the repository or organization settings UI.
