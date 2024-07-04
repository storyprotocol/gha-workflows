# Workflow Configuration

## Name

Reusable secret scanning workflow

## Triggers

 - Workflow Call

## Inputs

- Branch (string): required
- Depth (number): required, default is 2
  
## Secrets

- SLACK_BOT_TOKEN (required)
- SLACK_CHANNEL_ID_GITHUB_NOTIFICATION (required)
