# GitHub Actions Best Practices

## Workflow Design

### 1. Keep Workflows Focused

- Each workflow should have a single responsibility
- Separate CI, CD, and security scanning into different workflows
- Use reusable workflows for common patterns

### 2. Optimize Performance

```yaml
# Use caching for dependencies
- name: Cache node modules
  uses: actions/cache@v3
  with:
    path: ~/.npm
    key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
    restore-keys: |
      ${{ runner.os }}-node-
```

### 3. Security First

* Never hardcode secrets in workflow files
* Use minimal permissions with permissions key
* Scan for vulnerabilities in dependencies and code

## Composite Actions

### 1. Clear Inputs and Outputs

```yaml
inputs:
  node-version:
    description: 'Node.js version'
    required: false
    default: '18'
outputs:
  cache-hit:
    description: 'Whether cache was restored'
    value: ${{ steps.cache.outputs.cache-hit }}
```

### 2. Proper Error Handling

```yaml
- name: Run tests
  run: npm test
  continue-on-error: false  # Fail fast on errors
```

## Environment Management

### 1. Use Environment-specific Configurations

```yaml
deploy-staging:
  environment: staging
  runs-on: ubuntu-latest

deploy-production:
  environment: production
  runs-on: ubuntu-latest
  needs: deploy-staging
```

### 2. Implement Approval Gates

```yaml
- name: Wait for production deployment approval
  uses: trstringer/manual-approval@v1
  with:
    secret: ${{ github.TOKEN }}
    approvers: ${{ vars.PRODUCTION_APPROVERS }}
```

## Monitoring and Notifications

### 1. Comprehensive Status Reporting

```yaml
- name: Notify on failure
  if: failure()
  uses: actions/github-script@v6
  with:
    script: |
      // Create issue or comment on PR
```

### 2. Artifact Management

```yaml
- name: Upload test results
  uses: actions/upload-artifact@v3
  with:
    name: test-results
    path: results/
    retention-days: 30
```

## Performance Optimization

### 1. Matrix Strategies

```yaml
strategy:
  matrix:
    node-version: [14.x, 16.x, 18.x]
    os: [ubuntu-latest, windows-latest]
```

### 2. Dependency Caching

* Cache package manager directories
* Cache build outputs when appropriate
* Use exact key matches for primary caches

## Security Considerations

### 1. Minimal Permissions

```yaml
permissions:
  contents: read
  packages: write
  actions: read
```

### 2. Secret Management

* Use GitHub Secrets for all sensitive data
* Rotate secrets regularly
* Audit secret usage

## Testing and Validation

### 1. Local Testing

```bash
# Use act for local testing
act -s GITHUB_TOKEN=<token> -P ubuntu-latest=node:18-buster-slim
```

### 2. Workflow Linting

```yaml
# Add workflow linting to your CI
- name: Super-linter
  uses: github/super-linter@v4
  env:
    DEFAULT_BRANCH: main
    GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```