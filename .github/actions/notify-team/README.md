# Notify Team Action

A composite GitHub Action that sends notifications to multiple platforms (Slack, Discord, Email) with rich formatting and status awareness.

## Usage

### Basic Slack Notification

```yaml
steps:
  - name: Notify Team
    uses: your-username/github-actions-templates/.github/actions/notify-team@main
    with:
      status: ${{ job.status }}
      message: 'Build completed successfully'
      webhook-url: ${{ secrets.SLACK_WEBHOOK_URL }}
```

### Discord Notification

```yaml
steps:
  - name: Notify Team on Discord
    uses: your-username/github-actions-templates/.github/actions/notify-team@main
    with:
      status: success
      message: 'Deployment to production completed'
      webhook-url: ${{ secrets.DISCORD_WEBHOOK_URL }}
      platform: discord
```

### Email Notification

```yaml
steps:
  - name: Send Email Notification
    uses: your-username/github-actions-templates/.github/actions/notify-team@main
    with:
      status: failure
      message: 'Build failed - attention required'
      platform: email
      channel: 'team@company.com'
```

## Inputs

| Input	        | Description	                                                    | Required	| Default   |
|---------------|-------------------------------------------------------------------|-----------|-----------|
| status	    | Status of the notification (success, failure, cancelled, warning)	| Yes	    | -         |
| message	    | Main message to send	                                            | Yes	    | -         |
| webhook-url	| Webhook URL for Slack/Discord	                                    | No	    | -         |
| channel	    | Channel ID (Slack) or email address (Email)	                    | No	    | -         |
| platform	    | Platform to notify (slack, discord, email)	                    | No	    | slack     |
---

## Features

### Multi-Platform Support

* Slack: Rich messages with status icons and context
* Discord: Embedded messages with GitHub context
* Email: Professional email notifications with full details

### Status Awareness

* Automatically selects appropriate icons and colors
* Handles different status types consistently
* Provides contextual information

### Rich Context

* Includes automatic context about:
* Repository name and workflow
* Commit SHA and actor
* Direct link to workflow run
* Timestamp and duration

### Flexible Configuration

* Works with existing webhook configurations
* Supports multiple notification channels
* Customizable message formatting

## Examples

### Conditional Notifications

```yaml
- name: Notify on Failure
  if: failure()
  uses: your-username/github-actions-templates/.github/actions/notify-team@main
  with:
    status: failure
    message: 'Build failed in ${{ github.workflow }}'
    webhook-url: ${{ secrets.SLACK_WEBHOOK_URL }}

- name: Notify on Success
  if: success()
  uses: your-username/github-actions-templates/.github/actions/notify-team@main
  with:
    status: success
    message: 'Build succeeded in ${{ github.workflow }}'
    webhook-url: ${{ secrets.SLACK_WEBHOOK_URL }}
```

### Deployment Notifications

```yaml
- name: Notify Deployment Start
  uses: your-username/github-actions-templates/.github/actions/notify-team@main
  with:
    status: warning
    message: 'Starting deployment to ${{ inputs.environment }}'
    webhook-url: ${{ secrets.SLACK_WEBHOOK_URL }}

- name: Notify Deployment Complete
  if: success()
  uses: your-username/github-actions-templates/.github/actions/notify-team@main
  with:
    status: success
    message: 'Successfully deployed to ${{ inputs.environment }}'
    webhook-url: ${{ secrets.SLACK_WEBHOOK_URL }}
```

### Multi-Channel Notifications

```yaml
- name: Notify Slack Channel
  uses: your-username/github-actions-templates/.github/actions/notify-team@main
  with:
    status: ${{ job.status }}
    message: 'Security scan completed'
    webhook-url: ${{ secrets.SLACK_SECURITY_WEBHOOK }}
    channel: '#security-alerts'

- name: Notify Discord
  uses: your-username/github-actions-templates/.github/actions/notify-team@main
  with:
    status: ${{ job.status }}
    message: 'Security scan completed'
    webhook-url: ${{ secrets.DISCORD_WEBHOOK }}
    platform: discord
```

## Platform-Specific Features

### Slack

* Uses Slack's webhook API
* Supports channel overrides
* Rich message formatting with emojis
* Interactive message components

### Discord

* Discord webhook integration
* Embedded message format
* Color-coded status messages
* GitHub context integration

### Email

* SMTP-based email delivery
* Professional email formatting
* HTML and plain text versions
* Configurable SMTP servers

## Setup Requirements

### Slack Setup

1. Create a Slack app in your workspace
2. Enable Incoming Webhooks
3. Create a webhook URL for your channel
4. Add the webhook URL to your GitHub secrets

### Discord Setup

1. Create a webhook in your Discord channel settings
2. Copy the webhook URL
3. Add the webhook URL to your GitHub secrets

### Email Setup

1. Configure SMTP server details in GitHub secrets:
    * SMTP_SERVER
    * SMTP_PORT
    * SMTP_USERNAME
    * SMTP_PASSWORD
2. Specify recipient email in the channel input

## Best Practices

### Security

* Store webhook URLs and credentials in GitHub Secrets
* Use minimal required permissions for webhooks
* Rotate credentials regularly

### Message Design

* Keep messages concise but informative
* Include relevant context and links
* Use appropriate status indicators
* Consider your audience

### Error Handling

* Notifications fail gracefully if platforms are unavailable
* Continue workflow execution even if notification fails
* Log notification attempts for debugging

### Rate Limiting

* Be mindful of platform rate limits
* Avoid spammy notification patterns
* Use conditional notifications appropriately

## Troubleshooting

### Notifications Not Sending

* Verify webhook URLs are correct and active
* Check GitHub Secrets are properly configured
* Review workflow logs for error messages
* Test webhook URLs independently

### Formatting Issues

* Check special character handling
* Verify message length limits
* Test with different status types
* Review platform-specific formatting rules

### Platform-Specific Issues

* Slack: Verify app permissions and channel access
* Discord: Check webhook is still valid and has permissions
* Email: Verify SMTP settings and authentication

### Contributing

To contribute to this action:
1. Test with all supported platforms
2. Maintain consistent formatting across platforms
3. Add new platform integrations when requested
4. Update documentation for new features

## License

This action is part of the GitHub Actions Templates repository and is licensed under the MIT License.