# Reusable GitHub Action Workflows

This repository contains a comprehensive collection of reusable GitHub Action workflows designed to streamline CI/CD, security, testing, and operational processes across Story Protocol projects.

## Quick Start

To use any of these workflows in your repository:

```yaml
jobs:
  my-job:
    uses: storyprotocol/gha-workflows/.github/workflows/<workflow-filename>@main
    with:
      # workflow inputs
    secrets:
      # workflow secrets
```

## Workflow Categories

### 🛠️ Utility Workflows

General-purpose workflows for common operations.

| Workflow | Purpose | Key Features | When to Use |
|----------|---------|--------------|-------------|
| [reusable-timestamp.yml](docs/utility/readme-reusable-timestamp.md) | Generate timestamps in specific timezones | • Configurable timezone<br>• Environment variable output<br>• Useful for versioning | Creating time-based identifiers, logging execution times |
| [reusable-lint-go-workflow.yml](docs/utility/readme-reusable-lint-go-workflow.md) | Lint Go code with golangci-lint | • Configurable Go version<br>• 5-minute timeout<br>• Latest golangci-lint | Ensuring Go code quality in PRs and main branch |
| [reusable-slack-notifs.yml](docs/utility/readme-reusable-slack-notifs.md) | Send formatted Slack notifications | • Rich message formatting<br>• Optional image support<br>• Markdown support | Build notifications, deployment alerts, failure alerts |

### 🚀 CI/CD Workflows

Build, test, and deployment workflows for various platforms and technologies.

#### Container Build & Push

| Workflow | Purpose | Key Features | When to Use |
|----------|---------|--------------|-------------|
| [reusable-ecr-build-push.yml](docs/cicd/readme-reusable-ecr-build-push.md) | Build and push Go API Docker images to AWS ECR | • OIDC authentication<br>• Go test execution<br>• Trivy scanning<br>• SHA + latest tags | Deploying Go API services to AWS |
| [reusable-ecr-build-push-temporal-worker.yml](docs/cicd/readme-reusable-ecr-build-push-temporal-worker.md) | Build and push Temporal worker images to AWS ECR | • Custom image tags<br>• OIDC authentication<br>• Temporal-specific Dockerfile | Deploying Temporal workers to AWS |
| [reusable-gcp-image-build-worker.yml](docs/cicd/readme-reusable-gcp-build-push.md) | Build and push Docker images to GCP Container Registry | • Environment-aware<br>• Flexible Dockerfile paths<br>• Environment variables support | Deploying containerized apps to GCP |
| [reusable-gcp-app-release-publisher.yml](docs/cicd/readme-reusable-gcp-app-release-publisher.md) | Publish release messages to GCP Pub/Sub | • Triggers deployments<br>• Environment mapping<br>• Structured JSON messages | Triggering GCP deployments via Pub/Sub |

#### Testing Workflows

| Workflow | Purpose | Key Features | When to Use |
|----------|---------|--------------|-------------|
| [reusable-build-test-workflow.yml](docs/cicd/readme-reusable-build-test-workflow.md) | Full build and test for Node.js projects | • Mochawesome reports<br>• GitHub Pages deployment<br>• Test artifacts | Comprehensive testing with visual reports |
| [reusable-build-unit-test-workflow.yml](docs/cicd/readme-reusable-build-unit-test-workflow.md) | Unit testing for Node.js projects | • Fast execution<br>• No report generation<br>• PR-friendly | Quick unit test validation |
| [reusable-build-integration-test-workflow.yml](docs/cicd/readme-reusable-build-integration-test-workflow.md) | Integration testing for Node.js projects | • Isolated integration tests<br>• Environment-specific | Testing external integrations |
| [reusable-build-python-unit-test-workflow.yml](docs/cicd/readme-reusable-build-python-unit-test-workflow.md) | Unit testing for Python projects | • Multi-version testing<br>• Coverage reports<br>• Virtual environments | Python service testing |
| [reusable-build-python-integration-test-workflow.yml](docs/cicd/readme-reusable-build-python-integration-test-workflow.md) | Integration testing for Python projects | • pytest-based<br>• Coverage included<br>• RPC testing support | Python integration validation |

#### Release Management

| Workflow | Purpose | Key Features | When to Use |
|----------|---------|--------------|-------------|
| [reusable-create-release.yml](docs/cicd/readme-reusable-create-release.md) | Create GitHub releases with changelogs | • Automated changelog<br>• Tag creation<br>• Previous tag detection | Creating versioned releases |

### 🔒 Security Workflows

Security scanning, access management, and infrastructure security workflows.

#### Scanning & Monitoring

| Workflow | Purpose | Key Features | When to Use |
|----------|---------|--------------|-------------|
| [reusable-secrets-scanning.yml](docs/security/readme-reusable-secrets-scanning.md) | Scan for hardcoded secrets with TruffleHog | • Verified secrets only<br>• Slack notifications<br>• Configurable depth | Preventing secret leaks |
| [secrets-scanning.yml](docs/security/readme-reusable-secrets-scanning.md) | Automatic secrets scanning on main branch | • Auto-triggers on push<br>• Calls reusable workflow | Continuous security monitoring |
| [scorecards.yml](docs/security/readme-reusable-scorecards.md) | OSSF Scorecards security assessment | • SARIF format output<br>• Code scanning integration<br>• Manual trigger | Security posture assessment |

#### AWS Infrastructure Security

| Workflow | Purpose | Key Features | When to Use |
|----------|---------|--------------|-------------|
| [reusable-fetch-bastion-ips.yml](docs/security/readme-reusable-fetch-bastion-ips.md) | Fetch bastion host IP addresses | • Multi-environment<br>• Conditional fetching<br>• EC2 tag filtering | Getting bastion IPs for access |
| [reusable-fetch-network-node-ips.yml](docs/security/readme-reusable-fetch-network-node-ips.md) | Fetch all network node IPs across regions | • Multi-region support<br>• Bulk IP retrieval<br>• Node metadata | Network-wide IP discovery |
| [reusable-fetch-security-group-ids.yml](docs/security/readme-reusable-fetch-security-group-ids.md) | Fetch security group IDs | • Multi-region<br>• Bastion + network SGs<br>• Region mapping | Dynamic SG management |
| [reusable-parse-bastion-access-files.yml](docs/security/readme-reusable-parse-bastion-access-files.md) | Parse YAML access configs to JSON | • YAML to JSON conversion<br>• IP permissions format<br>• Multi-environment | Preparing access rules |
| [reusable-update-security-groups.yml](docs/security/readme-reusable-update-security-groups.md) | Update security groups with new IPs | • Bulk updates<br>• Status tracking<br>• Multi-region | Adding access rules |
| [reusable-revoke-inbound-rules.yml](docs/security/readme-reusable-revoke-inbound-rules.md) | Revoke all inbound rules from SGs | • Clean slate approach<br>• Multi-region<br>• Conditional execution | Resetting security groups |
| [reusable-remove-gha-ip-from-sg.yml](docs/security/readme-reusable-remove-gha-ip-from-sg.md) | Remove GitHub Actions runner IPs | • Cleanup automation<br>• Multi-SG support<br>• Security hygiene | Post-deployment cleanup |

### 🧪 QA Workflows

Quality assurance and code coverage workflows.

| Workflow | Purpose | Key Features | When to Use |
|----------|---------|--------------|-------------|
| [reusable-forge-code-coverage.yml](docs/qa/readme-reusable-forge-code-coverage.md) | Generate code coverage for Solidity contracts | • Foundry/Forge integration<br>• lcov + HTML reports<br>• Path exclusion<br>• Branch coverage | Smart contract test coverage |

## Common Usage Patterns

### 1. Complete CI/CD Pipeline

```yaml
name: CI/CD Pipeline
on:
  push:
    branches: [main]
  pull_request:

jobs:
  lint:
    uses: storyprotocol/gha-workflows/.github/workflows/reusable-lint-go-workflow.yml@main
    with:
      go-version: '1.22'

  test:
    uses: storyprotocol/gha-workflows/.github/workflows/reusable-build-test-workflow.yml@main
    with:
      sha: ${{ github.sha }}
      ENVIRONMENT: staging
    secrets:
      WALLET_PRIVATE_KEY: ${{ secrets.WALLET_PRIVATE_KEY }}
      TEST_WALLET_ADDRESS: ${{ secrets.TEST_WALLET_ADDRESS }}

  security-scan:
    uses: storyprotocol/gha-workflows/.github/workflows/reusable-secrets-scanning.yml@main
    with:
      branch: ${{ github.ref_name }}
    secrets:
      SLACK_BOT_TOKEN: ${{ secrets.SLACK_BOT_TOKEN }}
      SLACK_CHANNEL_ID_GITHUB_NOTIFICATION: ${{ secrets.SLACK_CHANNEL_ID }}

  build-and-push:
    needs: [lint, test, security-scan]
    if: github.ref == 'refs/heads/main'
    uses: storyprotocol/gha-workflows/.github/workflows/reusable-ecr-build-push.yml@main
    with:
      ecr-repo: "my-api-service"
      ecr-repo-aws-region: "us-east-1"
    secrets:
      AWS_ACCOUNT: ${{ secrets.AWS_ACCOUNT }}
      AWS_ACCOUNT_TARGET: ${{ secrets.AWS_ACCOUNT_TARGET }}

  notify:
    needs: [build-and-push]
    if: always()
    uses: storyprotocol/gha-workflows/.github/workflows/reusable-slack-notifs.yml@main
    with:
      title: ${{ needs.build-and-push.result == 'success' && '✅ Deployment Successful' || '❌ Deployment Failed' }}
      short-desc: 'Build ${{ github.sha }} has ${{ needs.build-and-push.result }}'
      img-alt-text: 'Build status'
    secrets:
      channel-name: ${{ secrets.SLACK_CHANNEL_ID }}
      slack-bot-token: ${{ secrets.SLACK_BOT_TOKEN }}
```

### 2. Security Group Management Flow

```yaml
name: Update Bastion Access
on:
  push:
    paths:
      - 'configs/bastion-access-*.yaml'

jobs:
  parse-configs:
    uses: storyprotocol/gha-workflows/.github/workflows/reusable-parse-bastion-access-files.yml@main
    with:
      devnet_file: "configs/bastion-access-devnet.yaml"
      testnet_file: "configs/bastion-access-testnet.yaml"

  fetch-sg-ids:
    uses: storyprotocol/gha-workflows/.github/workflows/reusable-fetch-security-group-ids.yml@main
    with:
      role_to_assume: ${{ secrets.AWS_ROLE }}
      aws_region: "us-east-1"
      # ... other inputs

  revoke-existing:
    needs: [fetch-sg-ids]
    uses: storyprotocol/gha-workflows/.github/workflows/reusable-revoke-inbound-rules.yml@main
    with:
      sg_ids_devnet: ${{ needs.fetch-sg-ids.outputs.security_group_ids_devnet }}
      # ... other inputs

  update-sgs:
    needs: [parse-configs, fetch-sg-ids, revoke-existing]
    uses: storyprotocol/gha-workflows/.github/workflows/reusable-update-security-groups.yml@main
    with:
      ips_devnet: ${{ needs.parse-configs.outputs.ips_devnet }}
      sg_ids_devnet: ${{ needs.fetch-sg-ids.outputs.security_group_ids_devnet }}
      # ... other inputs
```

### 3. Multi-Environment Testing

```yaml
name: Multi-Environment Tests
on: [push, pull_request]

jobs:
  unit-tests:
    strategy:
      matrix:
        environment: [staging, production]
    uses: storyprotocol/gha-workflows/.github/workflows/reusable-build-unit-test-workflow.yml@main
    with:
      sha: ${{ github.sha }}
      ENVIRONMENT: ${{ matrix.environment }}
    secrets:
      WALLET_PRIVATE_KEY: ${{ secrets[format('WALLET_PRIVATE_KEY_{0}', matrix.environment)] }}
      TEST_WALLET_ADDRESS: ${{ secrets[format('TEST_WALLET_ADDRESS_{0}', matrix.environment)] }}
```

## Best Practices

1. **Version Pinning**: All workflows use pinned action versions with SHA hashes for security. Regularly update these after security review.

2. **Secret Management**: 
   - Use environment-specific secrets
   - Pass secrets explicitly through the `secrets` context
   - Never hardcode sensitive information

3. **Conditional Execution**: Many workflows support conditional execution based on environment changes. Use this to optimize CI/CD time.

4. **Error Handling**: Most workflows will fail fast on errors. Consider using `if: always()` for cleanup or notification steps.

5. **Documentation**: Each workflow has its own documentation in the `docs/` directory. Keep these updated when modifying workflows.

## Requirements

- GitHub Actions must be enabled in your repository
- Appropriate secrets must be configured at the repository or organization level
- For AWS workflows: OIDC provider must be configured
- For GCP workflows: Service account keys must be available
- For Slack workflows: Slack app with bot token must be configured

## Contributing

When adding new reusable workflows:

1. Follow the naming convention: `reusable-<purpose>.yml`
2. Include comprehensive input validation
3. Use pinned action versions with SHA hashes
4. Create documentation in `docs/<category>/readme-reusable-<name>.md`
5. Update this README with the new workflow information
6. Test thoroughly in a separate repository before merging

## Security

- All workflows use pinned action versions for security
- Secrets are handled through GitHub's encrypted secrets
- AWS workflows use OIDC for temporary credentials
- Regular security scanning with TruffleHog and OSSF Scorecards

## License

[Include your license information here]