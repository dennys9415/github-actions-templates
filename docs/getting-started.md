# Getting Started with GitHub Actions Templates

## Overview

This repository provides reusable GitHub Actions workflows and composite actions that you can use in your projects to set up robust CI/CD pipelines quickly.

## Quick Start

### 1. Using Workflow Templates

**Option A: Copy the workflow file**

```bash
mkdir -p .github/workflows
curl -o .github/workflows/ci.yml \
  https://raw.githubusercontent.com/your-username/github-actions-templates/main/.github/workflows/ci.yml
```

**Option B: Use the reusable workflow**

```yaml
# In your .github/workflows/ci.yml
name: CI

on: [push, pull_request]

jobs:
  test:
    uses: your-username/github-actions-templates/.github/workflows/ci.yml@main
    with:
      node-version: '18'
    secrets: inherit
```

### 2. Using Composite Actions

```yaml
steps:
  - name: Setup Environment
    uses: your-username/github-actions-templates/.github/actions/setup-environment@main
    with:
      node-version: '18'
      python-version: '3.11'

  - name: Notify Team
    uses: your-username/github-actions-templates/.github/actions/notify-team@main
    with:
      status: ${{ job.status }}
      message: 'Build completed'
      webhook-url: ${{ secrets.SLACK_WEBHOOK }}
```

### Configuration

### Environment Variables

Set these in your repository secrets:

* SLACK_WEBHOOK_URL: For notifications
* SNYK_TOKEN: For security scanning
* NPM_TOKEN: For publishing packages

### Customization

Each template is designed to be customizable:

```yaml
- name: Setup Environment
  uses: your-username/github-actions-templates/.github/actions/setup-environment@main
  with:
    node-version: '20'  # Custom Node version
    install-deps: 'false'  # Skip dependency installation
```

### Best Practices

1. Start Simple: Begin with the CI template, then add security scanning and deployment
2. Use Matrix Builds: Test across multiple versions and platforms
3. Enable Caching: Speed up your workflows with proper caching
4. Set Up Notifications: Keep your team informed of build status
5. Secure Your Secrets: Use GitHub Secrets for sensitive data

### Next Steps

* Check out the examples for language-specific configurations
* Read about best practices
* Learn how to contribute