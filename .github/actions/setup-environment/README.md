# Setup Environment Action

A composite GitHub Action that sets up a consistent development environment with common tools, dependency management, and caching.

## Usage

### Basic Usage

```yaml
steps:
  - name: Setup Environment
    uses: your-username/github-actions-templates/.github/actions/setup-environment@main
```

## Advanced Usage

```yaml
steps:
  - name: Setup Environment
    uses: your-username/github-actions-templates/.github/actions/setup-environment@main
    with:
      node-version: '20'
      python-version: '3.12'
      install-deps: 'true'
      cache-enabled: 'true'
```

## Inputs

| Input	            | Description	                    | Required  | Default   |
|-------------------|-----------------------------------|-----------|-----------|
| node-version	    | Node.js version to setup	        | No	    | '18'      |
| python-version	| Python version to setup	        | No	    | '3.11'    |
| install-deps	    | Whether to install dependencies	| No	    | 'true'    |
| cache-enabled	    | Whether to enable caching	        | No	    | 'true'    |
---

## Outputs

| Output	        | Description                               |
|-------------------|-------------------------------------------|
| node-version	    | The Node.js version that was set up       |
| python-version	| The Python version that was set up        |
| cache-hit	        | Whether cache was restored (true/false)   |
---

## Features

### Multi-Language Support

* Automatically detects and sets up Node.js and Python environments
* Supports multiple version specifications
* Handles both npm and pip dependency management

### Smart Caching

* Caches npm and pip dependencies
* Uses content-based cache keys for optimal cache hits
* Supports cache restoration and fallback

### Dependency Management

* Automatically installs dependencies if package files are detected
* Supports both package.json and requirements.txt
* Handles development dependencies appropriately


### Verification

* Verifies all installations
* Provides version information in logs
* Validates environment setup

## Examples

### Node.js Project

```yaml
steps:
  - name: Setup Node.js Environment
    uses: your-username/github-actions-templates/.github/actions/setup-environment@main
    with:
      node-version: '18'
      python-version: ''  # Disable Python
```

### Python Project

```yaml
steps:
  - name: Setup Python Environment
    uses: your-username/github-actions-templates/.github/actions/setup-environment@main
    with:
      node-version: ''  # Disable Node.js
      python-version: '3.11'
```

### Multi-language Project

```yaml
steps:
  - name: Setup Full Environment
    uses: your-username/github-actions-templates/.github/actions/setup-environment@main
    with:
      node-version: '18'
      python-version: '3.11'
      install-deps: 'true'
```

### Skip Dependency Installation

```yaml
steps:
  - name: Setup Environment Without Dependencies
    uses: your-username/github-actions-templates/.github/actions/setup-environment@main
    with:
      install-deps: 'false'
```

## Cache Behavior

The action implements intelligent caching:

1. Cache Key: Based on OS, package manager, and dependency file hashes
2. Cache Restoration: Attempts to restore from exact match first, then partial matches
3. Fallback: If cache miss, installs dependencies fresh and creates new cache

## Error Handling

* Missing Tools: Fails gracefully with clear error messages
* Dependency Issues: Continues on best-effort basis
* Cache Failures: Falls back to fresh installation

## Best Practices

1. Pin Versions: Always specify exact versions for production workflows
2. Enable Caching: Keep caching enabled for faster builds
3. Monitor Cache Size: Be mindful of cache storage limits
4. Update Regularly: Keep Node.js and Python versions updated

## Troubleshooting

### Cache Not Working

* Verify dependency files haven't changed significantly
* Check cache storage limits in repository settings
* Ensure cache keys are consistent

### Version Conflicts

* Specify exact versions rather than ranges
* Use version that's available in GitHub Actions runners
* Check action logs for available versions

### Dependency Installation Issues

* Verify package files are in the correct location
* Check for syntax errors in package files
* Ensure all required files are committed to repository

### Contributing

To contribute to this action:
1. Follow the composite action guidelines
2. Test with different project configurations
3. Update documentation for new features
4. Maintain backward compatibility

## License

This action is part of the GitHub Actions Templates repository and is licensed under the MIT License.

