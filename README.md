# Reusable GitHub Action Workflows

[![Workflow Lint](https://img.shields.io/github/actions/workflow/status/storyprotocol/gha-workflows/lint-validation.yml?branch=main&label=workflow%20lint)](https://github.com/storyprotocol/gha-workflows/actions/workflows/lint-validation.yml)
[![Secrets Scan](https://img.shields.io/github/actions/workflow/status/storyprotocol/gha-workflows/secrets-scanning.yml?branch=main&label=secrets%20scan)](https://github.com/storyprotocol/gha-workflows/actions/workflows/secrets-scanning.yml)
[![pre-commit](https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit)](.pre-commit-config.yaml)
[![Stars](https://img.shields.io/github/stars/storyprotocol/gha-workflows?style=flat&label=stars)](https://github.com/storyprotocol/gha-workflows/stargazers)
[![Forks](https://img.shields.io/github/forks/storyprotocol/gha-workflows?style=flat&label=forks)](https://github.com/storyprotocol/gha-workflows/forks)

This repository contains reusable GitHub Actions workflows used across Story
Protocol repositories, plus a small set of workflows that maintain this
repository itself.

The source of truth is `.github/workflows/`. Workflow inputs and outputs are
defined under `on.workflow_call`; some older workflows also reference repository
secrets or variables directly.

## Quick Start

Use a reusable workflow from another repository with `jobs.<job_id>.uses`:

```yaml
jobs:
  lint:
    uses: storyprotocol/gha-workflows/.github/workflows/reusable-lint-go-workflow.yml@main
    with:
      go-version: "1.22"
```

For workflows that require secrets, pass the named secrets explicitly or use
`secrets: inherit` only when that is allowed by the caller repository's policy.

```yaml
jobs:
  notify:
    uses: storyprotocol/gha-workflows/.github/workflows/reusable-slack-notifs.yml@main
    with:
      title: "Build completed"
      short-desc: "The build finished successfully."
      img-alt-text: "Build status"
    secrets:
      channel-name: ${{ secrets.SLACK_CHANNEL_ID }}
      slack-bot-token: ${{ secrets.SLACK_BOT_TOKEN }}
```

## Workflow Index

### Utility

| Workflow | Purpose |
| --- | --- |
| [`reusable-lint-go-workflow.yml`](.github/workflows/reusable-lint-go-workflow.yml) | Sets up Go and runs `golangci-lint run --timeout 5m`. |
| [`reusable-slack-notifs.yml`](.github/workflows/reusable-slack-notifs.yml) | Sends Slack Block Kit notifications, with optional image support. |
| [`reusable-timestamp.yml`](.github/workflows/reusable-timestamp.yml) | Generates and prints a timestamp for a configurable timezone. |

### CI/CD and Release

| Workflow | Purpose |
| --- | --- |
| [`reusable-build-and-deploy.yml`](.github/workflows/reusable-build-and-deploy.yml) | Builds and pushes an ECR image, then updates image tags in a deployment config repository. |
| [`reusable-create-release.yml`](.github/workflows/reusable-create-release.yml) | Creates an annotated Git tag and GitHub release with a generated changelog. |
| [`reusable-ecr-build-push.yml`](.github/workflows/reusable-ecr-build-push.yml) | Runs `go test ./api/...`, builds `./dockerfile/api/Dockerfile`, and pushes SHA/latest tags to ECR. |
| [`reusable-ecr-build-push-temporal-worker.yml`](.github/workflows/reusable-ecr-build-push-temporal-worker.yml) | Builds `./dockerfile/temporal-worker/Dockerfile` and pushes a custom image tag plus `latest` to ECR. |
| [`reusable-gcp-app-release-publisher.yml`](.github/workflows/reusable-gcp-app-release-publisher.yml) | Publishes a release message to the configured GCP Pub/Sub courier topic. |
| [`reusable-gcp-image-build-worker.yml`](.github/workflows/reusable-gcp-image-build-worker.yml) | Builds and pushes a Docker image to GCR for staging or production. |

### Testing

| Workflow | Purpose |
| --- | --- |
| [`reusable-build-integration-test-workflow.yml`](.github/workflows/reusable-build-integration-test-workflow.yml) | Runs Node/pnpm integration tests with `pnpm run test:integration`, then builds. |
| [`reusable-build-python-integration-test-workflow.yml`](.github/workflows/reusable-build-python-integration-test-workflow.yml) | Runs Python 3.10/3.11 integration tests with pytest coverage. |
| [`reusable-build-python-unit-test-workflow.yml`](.github/workflows/reusable-build-python-unit-test-workflow.yml) | Runs Python 3.10/3.11 unit tests with pytest coverage. |
| [`reusable-build-test-workflow.yml`](.github/workflows/reusable-build-test-workflow.yml) | Runs Node/pnpm fix, test, and build steps, then uploads and publishes a Mochawesome report. |
| [`reusable-build-unit-test-workflow.yml`](.github/workflows/reusable-build-unit-test-workflow.yml) | Runs Node/pnpm fix, test, and build steps without report publishing. |

### Security and Access Management

| Workflow | Purpose |
| --- | --- |
| [`reusable-fetch-bastion-ips.yml`](.github/workflows/reusable-fetch-bastion-ips.yml) | Fetches DevNet/TestNet bastion public IPs from EC2 when related files changed. |
| [`reusable-fetch-network-node-ips.yml`](.github/workflows/reusable-fetch-network-node-ips.yml) | Fetches DevNet/TestNet node IP data across caller-provided AWS regions. |
| [`reusable-fetch-security-group-ids.yml`](.github/workflows/reusable-fetch-security-group-ids.yml) | Finds DevNet/TestNet security group IDs and bastion security group IDs. |
| [`reusable-parse-bastion-access-files.yml`](.github/workflows/reusable-parse-bastion-access-files.yml) | Parses DevNet/TestNet bastion access YAML files into JSON permissions. |
| [`reusable-remove-gha-ip-from-sg.yml`](.github/workflows/reusable-remove-gha-ip-from-sg.yml) | Removes a GitHub Actions runner IP from DevNet/TestNet security groups and bastion security groups. |
| [`reusable-revoke-inbound-rules.yml`](.github/workflows/reusable-revoke-inbound-rules.yml) | Revokes inbound rules from DevNet/TestNet security groups. |
| [`reusable-secrets-scanning.yml`](.github/workflows/reusable-secrets-scanning.yml) | Runs TruffleHog with `--results=verified` and sends a Slack notification on failure. |
| [`reusable-update-security-groups.yml`](.github/workflows/reusable-update-security-groups.yml) | Adds parsed IP permissions to DevNet/TestNet security groups. |
| [`scorecards.yml`](.github/workflows/scorecards.yml) | Manually runs OSSF Scorecards and uploads SARIF results to code scanning. |
| [`secrets-scanning.yml`](.github/workflows/secrets-scanning.yml) | Calls the reusable TruffleHog scan on pushes to `main`. |

### QA

| Workflow | Purpose |
| --- | --- |
| [`reusable-forge-code-coverage.yml`](.github/workflows/reusable-forge-code-coverage.yml) | Runs Foundry coverage, filters paths with lcov, generates HTML, and uploads the coverage artifact. |

### Repository Maintenance

| File | Purpose |
| --- | --- |
| [`lint-validation.yml`](.github/workflows/lint-validation.yml) | Lints workflow YAML files and performs a basic workflow-name validation on workflow changes. |
| [`dependabot.yml`](.github/dependabot.yml) | Checks GitHub Actions dependencies daily. |
| [`.pre-commit-config.yaml`](.pre-commit-config.yaml) | Configures the `gitleaks` pre-commit hook. |

## Caller Requirements

These workflows assume the caller repository supplies the expected repository
layout, tools, variables, and secrets.

- AWS ECR workflows require an OIDC-capable AWS role or the expected
  `AWS_ACCOUNT` / `AWS_ACCOUNT_TARGET` secrets, depending on the workflow.
- `reusable-build-and-deploy.yml` requires `AWS_ROLE_ARN` and `DEPLOY_TOKEN`,
  and writes image tag updates to a deployment config repository.
- GCP workflows require the referenced service account secrets and Pub/Sub
  variables, such as `COURIER_SERVICE_ACCOUNT_KEY` and `COURIER_TOPIC`.
- Slack workflows require a Slack channel ID/name and bot token.
- Node test workflows assume pnpm 8 and Node 20.
- Python test workflows assume `requirements.txt` and `tests/unit` or
  `tests/integration` exist in the caller repository.
- Some workflows assume Story-specific paths, such as `./dockerfile/api`,
  `./dockerfile/temporal-worker`, and `packages/core-sdk/mochawesome-report`.

The AWS access-management workflows call helper scripts from the checked-out
caller repository. Callers must provide the scripts used by the selected
workflow:

- `scripts/parse_config.py`
- `scripts/fetch_all_node_ips.sh`
- `scripts/get_sg_id_by_name.sh`
- `scripts/update_security_group.sh`
- `scripts/revoke_inbound_rules.sh`
- `scripts/remove_ip_from_sg.sh`

## Validation

This repository validates workflow changes through
[`lint-validation.yml`](.github/workflows/lint-validation.yml), which runs
yamllint using [`.github/linters/.yamllint.yml`](.github/linters/.yamllint.yml)
and checks each workflow file has a `name` field.

When changing a reusable workflow, also test it from a caller repository or a
temporary workflow before merging.

## Contributing

When adding or changing a reusable workflow:

1. Put the workflow in `.github/workflows/`.
2. Keep the `workflow_call` inputs, outputs, and secrets explicit.
3. Prefer SHA-pinned third-party actions when practical, and document any
   intentional exception.
4. Update this README with the workflow's current behavior.
5. Add or update detailed docs under `docs/` only when they are kept in sync
   with the workflow.
