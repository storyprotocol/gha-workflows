# Workflow Configuration

## Name

Reusable workflow to create slack notifications

## Triggers

The workflow is triggered using workflow_call, allowing it to be reused in other workflows.

## Inputs

- short-desc (string, required): A brief description of the Slack message.
- title (string, required): The title of the Slack message.
- img-url (string, required): URL of the image to be included in the Slack message. Requires downloadable image file (.png, .jpg, etc.)
- img-alt-text (string, required): Alt text for the image.
  
## Secrets

- channel-name (required): The name of the Slack channel where the message will be sent.
- slack-bot-token (required): The token for the Slack bot to authenticate the API request.
