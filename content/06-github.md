# GitHub Actions

## Introduction

GitHub Actions is a CI/CD and automation platform integrated with GitHub repositories. Workloads in GitHub Actions are executed as jobs within workflows, running on GitHub-hosted or self-hosted runners.

| Workload Type | Description |
|--------------|-------------|
| Workflow     | A collection of jobs triggered by events. |
| Job          | A set of steps executed on a runner.      |
| Step         | An individual command or action.          |

- [GitHub Actions Documentation](https://docs.github.com/actions)

## Security Model

GitHub Actions uses a token-based security model. Each workflow run is provided with automatically generated tokens (such as `GITHUB_TOKEN`) that are scoped to the repository and have limited permissions. Additional secrets can be configured at the repository, organization, or environment level.

- **Authentication:** Workloads authenticate to the GitHub API and other services using the `GITHUB_TOKEN` or custom secrets.
- **Authorization:** The permissions of `GITHUB_TOKEN` and other secrets are defined by repository settings and can be further restricted using fine-grained permissions and environments.

## Workloads Identity and Permissions

In GitHub Actions, the identity of a workload is determined by the context in which it runs (repository, actor, and workflow). Permissions are managed as follows:

- **GITHUB_TOKEN:** Automatically generated for each workflow run, scoped to the repository, and with configurable permissions.
- **Custom Secrets:** Additional secrets (API keys, tokens) can be added to workflows and are only available to selected jobs or environments.
- **Fine-Grained Permissions:** Repository administrators can restrict the permissions of `GITHUB_TOKEN` and control access to secrets.

## Authentication and Authorisation for Platform APIs

Requests to the GitHub API from within Actions are authenticated using the `GITHUB_TOKEN` or personal access tokens (PATs). The `GITHUB_TOKEN` is injected into the workflow environment and used as a Bearer token in API requests.

- **Authentication:** Use the `GITHUB_TOKEN` or a secret token in the `Authorization: Bearer <token>` header for API requests.
- **Authorization:** GitHub checks the token's permissions and the repository's security settings to authorize the request.
- **Technical Implementation:**
    - The Actions runner injects the `GITHUB_TOKEN` and secrets as environment variables.
    - Example: A workflow step uses `curl` or the GitHub CLI with the `GITHUB_TOKEN` to call the GitHub API.

## Local Development

For local development and testing of GitHub Actions workflows:

- **act Tool:** Use the [`act`](https://github.com/nektos/act) tool to run workflows locally. You must provide your own tokens and secrets as environment variables.
- **Personal Access Tokens:** Use a personal access token (PAT) with appropriate permissions for API calls during local testing.
- **Environment Variables:** Set required secrets and tokens as environment variables when running workflows locally.

Never commit secrets or tokens to source code. Use the principle of least privilege and regularly rotate tokens and secrets.
