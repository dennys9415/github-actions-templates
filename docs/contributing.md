# Contributing to GitHub Actions Templates

First off, thank you for considering contributing to GitHub Actions Templates! It's people like you that make this collection of reusable workflows valuable for the community.

## Code of Conduct

By participating in this project, you are expected to uphold our [Code of Conduct](CODE_OF_CONDUCT.md).

## How Can I Contribute?

### Reporting Bugs

This section guides you through submitting a bug report. Following these guidelines helps maintainers understand your report, reproduce the behavior, and find related reports.

**Before Submitting A Bug Report**
* Check the [existing issues](../../issues) to see if the problem has already been reported.
* Perform a [cursory search](../../search?q=is%3Aissue+is%3Aopen+sort%3Aupdated-desc) to see if the issue has already been resolved.

**How Do I Submit A (Good) Bug Report?**
Bugs are tracked as [GitHub issues](../../issues). Create an issue and provide the following information:

- **Use a clear and descriptive title** for the issue to identify the problem.
- **Describe the exact steps which reproduce the problem** in as many details as possible.
- **Provide specific examples to demonstrate the steps**. Include links to files or GitHub projects, or copy/pasteable snippets, which you use in those examples.
- **Describe the behavior you observed after following the steps** and point out what exactly is the problem with that behavior.
- **Explain which behavior you expected to see instead and why.**
- **Include screenshots and animated GIFs** which show you following the described steps and clearly demonstrate the problem.

### Suggesting Enhancements

This section guides you through submitting an enhancement suggestion, including completely new features and minor improvements to existing functionality.

**Before Submitting An Enhancement Suggestion**
* Check if there's already [a feature request](../../issues?q=is%3Aopen+is%3Aissue+label%3Aenhancement) for what you want to suggest.

**How Do I Submit A (Good) Enhancement Suggestion?**
Enhancement suggestions are tracked as [GitHub issues](../../issues). Create an issue and provide the following information:

- **Use a clear and descriptive title** for the issue to identify the suggestion.
- **Provide a step-by-step description of the suggested enhancement** in as many details as possible.
- **Provide specific examples to demonstrate the steps** or point out the part of the template where the suggestion is related to.
- **Describe the current behavior** and **explain which behavior you expected to see instead** and why.
- **Explain why this enhancement would be useful** to most GitHub Actions Templates users.

### Pull Requests

- Fill in the required template
- Follow the styleguides
- After you submit your pull request, verify that all status checks are passing

## Development Setup

### Prerequisites

- Git
- A GitHub account
- Basic understanding of GitHub Actions

### Setting Up the Repository

1. Fork the repository on GitHub
2. Clone your fork locally:

```bash
git clone https://github.com/your-username/github-actions-templates.git
cd github-actions-templates
```

3. Create a branch for your changes:

```bash
git checkout -b feature/amazing-template
```

## Style Guides

### Git Commit Messages

* Use the present tense ("Add feature" not "Added feature")
* Use the imperative mood ("Move cursor to..." not "Moves cursor to...")
* Limit the first line to 72 characters or less
* Reference issues and pull requests liberally after the first line

### GitHub Actions Workflow Style Guide

* Use clear, descriptive names for workflows and jobs
* Include comments for complex steps
* Use consistent indentation (2 spaces)
* Group related steps together
* Use meaningful step names

### YAML Style Guide

* Use 2-space indentation
* Use kebab-case for file names
* Quote strings that contain special characters
* Use consistent naming conventions for inputs and outputs

## Template Development Guidelines

### Creating New Workflow Templates

1. Start with a clear purpose: Each template should solve a specific problem
2. Make it reusable: Use inputs and secrets for configuration
3. Include documentation: Add comments and update the README
4. Test thoroughly: Verify the workflow works in different scenarios
5. Follow security best practices: Use minimal permissions and handle secrets properly

### Example Workflow Structure

```yaml
name: Descriptive Workflow Name

on:
  # Define triggers

env:
  # Set default environment variables

jobs:
  job-name:
    name: Descriptive Job Name
    runs-on: ubuntu-latest
    
    steps:
    - name: Clear step description
      uses: action/name@version
      with:
        # Inputs
```

## Creating Composite Actions

### 1. Define clear inputs and outputs:

```yaml
inputs:
  node-version:
    description: 'Node.js version to use'
    required: false
    default: '18'
outputs:
  cache-hit:
    description: 'Whether the cache was restored'
    value: ${{ steps.cache.outputs.cache-hit }}
```

### 2. Handle errors gracefully:

```yaml
- name: Run command
  shell: bash
  run: |
    set -euo pipefail
    # Your commands here
```

### 3. Provide usage examples in the README

## Testing Your Changes

### Testing Workflows

1. Create a test repository or use your fork

2. Reference your branch in workflow files:

```yaml
jobs:
  test:
    uses: your-username/github-actions-templates/.github/workflows/ci.yml@your-branch
```

3. Trigger the workflow and verify it works as expected

## Testing Composite Actions

1. Create a test workflow that uses your action:

```yaml
steps:
  - name: Test my action
    uses: your-username/github-actions-templates/.github/actions/setup-environment@your-branch
```

## Submitting Your Changes

1. Ensure your changes work by testing them thoroughly
2. Update documentation including README.md and any relevant examples
3. Add or update tests if applicable
4. Create a pull request with a clear description of your changes

## Review Process

1. Maintainers will review your pull request
2. You may be asked to make changes
3. Once approved, your PR will be merged
4. Thank you for your contribution!

## Recognition

Contributors will be recognized in:

* The README.md file
* Release notes
* GitHub contributors graph

## Questions?

Feel free to:

* Open an issue with your question
* Start a discussion in the repository
* Contact the maintainers directly

Thank you for contributing to make GitHub Actions better for everyone! 🚀