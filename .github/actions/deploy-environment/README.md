# Deploy Environment Action

A composite GitHub Action that handles deployment to various environments (Kubernetes, AWS, etc.) with support for multiple deployment strategies and rollback capabilities.

## Usage

### Basic Kubernetes Deployment

```yaml
steps:
  - name: Deploy to Staging
    uses: your-username/github-actions-templates/.github/actions/deploy-environment@main
    with:
      environment: staging
      image-tag: ghcr.io/your-org/app:v1.2.3
      kube-config: ${{ secrets.STAGING_KUBECONFIG }}
```

### Advanced Deployment with Custom Manifest

```yaml
steps:
  - name: Deploy to Production
    uses: your-username/github-actions-templates/.github/actions/deploy-environment@main
    with:
      environment: production
      image-tag: ghcr.io/your-org/app:v1.2.3
      kube-config: ${{ secrets.PRODUCTION_KUBECONFIG }}
      namespace: production
      deployment-file: k8s/production-deployment.yaml
```

### Deployment with Verification

```yaml
steps:
  - name: Deploy and Verify
    uses: your-username/github-actions-templates/.github/actions/deploy-environment@main
    with:
      environment: staging
      image-tag: ${{ needs.build.outputs.image-tag }}
      kube-config: ${{ secrets.STAGING_KUBECONFIG }}
    id: deploy

  - name: Run Smoke Tests
    run: |
      curl -f ${{ steps.deploy.outputs.deployment-url }}/health
      npm run test:smoke -- --url=${{ steps.deploy.outputs.deployment-url }}
```

## Inputs

| Input	                | Description	                                    | Required	| Default               |
|-----------------------|---------------------------------------------------|-----------|-----------------------|
| environment	        | Target environment (staging, production, etc.)	| Yes	    | -                     |
| image-tag	            | Docker image tag to deploy	                    | Yes	    | -                     |
| kube-config	        | Kubernetes configuration (base64 encoded)	        | No	    | -                     |
| namespace	            | Kubernetes namespace	                            | No	    | default               |
| deployment-file	    | Path to deployment manifest	                    | No	    | k8s/deployment.yaml   |
| timeout	            | Deployment timeout in seconds	                    | No	    | 300                   |
| rollback-on-failure	| Enable automatic rollback on failure	            | No	    | true                  |
---

Outputs

| Output	        | Description                                   |
|-------------------|-----------------------------------------------|
| deployed-version	| The version that was deployed                 |
| deployment-url	| URL of the deployed application               |
| namespace	        | Namespace where deployment occurred           |
| deployment-status	| Status of the deployment (success, failure)   |
---

## Features

### Multi-Environment Support

* Consistent deployment across staging, production, and other environments
* Environment-specific configuration handling
* Separate credentials and namespaces per environment

### Kubernetes Integration

* Automatic kubeconfig setup and context switching
* Support for custom namespaces and deployment files
* Rollout status monitoring with configurable timeouts
* Automatic rollback on deployment failures

### Deployment Strategies

* Blue-Green: Zero-downtime deployments with traffic switching
* Canary: Gradual rollout with percentage-based traffic splitting
* Rolling Update: Default Kubernetes rolling update strategy

### Verification and Health Checks

* Automatic deployment status verification
* Health check endpoint validation
* Readiness and liveness probe support
* Custom verification script execution

## Examples

### Simple Kubernetes Deployment

```yaml
- name: Deploy to Kubernetes
  uses: your-username/github-actions-templates/.github/actions/deploy-environment@main
  with:
    environment: staging
    image-tag: ${{ secrets.REGISTRY }}/app:${{ github.sha }}
    kube-config: ${{ secrets.KUBECONFIG_STAGING }}
```

### Production Deployment with Approval

```yaml
- name: Wait for Approval
  uses: trstringer/manual-approval@v1
  with:
    secret: ${{ github.TOKEN }}
    approvers: ${{ vars.PRODUCTION_APPROVERS }}

- name: Deploy to Production
  uses: your-username/github-actions-templates/.github/actions/deploy-environment@main
  with:
    environment: production
    image-tag: ${{ needs.build.outputs.image-tag }}
    kube-config: ${{ secrets.KUBECONFIG_PRODUCTION }}
    namespace: production
    timeout: 600
```

### Custom Deployment Manifest

```yaml
- name: Deploy with Custom Config
  uses: your-username/github-actions-templates/.github/actions/deploy-environment@main
  with:
    environment: production
    image-tag: ghcr.io/company/app:v2.0.0
    kube-config: ${{ secrets.KUBECONFIG }}
    deployment-file: k8s/production/backend-deployment.yaml
    namespace: backend-services
```

### Multi-Service Deployment

```yaml
- name: Deploy Backend
  uses: your-username/github-actions-templates/.github/actions/deploy-environment@main
  with:
    environment: production
    image-tag: ghcr.io/company/backend:${{ github.sha }}
    kube-config: ${{ secrets.KUBECONFIG }}
    deployment-file: k8s/backend-deployment.yaml
  id: backend-deploy

- name: Deploy Frontend
  uses: your-username/github-actions-templates/.github/actions/deploy-environment@main
  with:
    environment: production
    image-tag: ghcr.io/company/frontend:${{ github.sha }}
    kube-config: ${{ secrets.KUBECONFIG }}
    deployment-file: k8s/frontend-deployment.yaml
  id: frontend-deploy
```

## Deployment Strategies

### Rolling Update (Default)

```yaml
# Uses Kubernetes native rolling update strategy
# Gradually replaces old pods with new ones
```

### Blue-Green Deployment

```yaml
- name: Blue-Green Deployment
  run: |
    # Deploy new version (green)
    kubectl apply -f deployment-green.yaml
    
    # Switch traffic
    kubectl apply -f service-green.yaml
    
    # Clean up old version (blue)
    kubectl delete -f deployment-blue.yaml
```

### Canary Deployment

```yaml
- name: Canary Deployment
  run: |
    # Deploy canary (10% traffic)
    kubectl apply -f canary-deployment.yaml
    
    # Monitor canary
    sleep 300
    
    # Roll out to 100% if successful
    kubectl apply -f full-deployment.yaml
```

## Setup Requirements

### Kubernetes Setup

1. Cluster Access: Configure kubeconfig for target clusters
2. Secrets Management: Store kubeconfig as GitHub secrets
3. RBAC: Ensure sufficient permissions for deployment
4. Namespace Configuration: Create necessary namespaces

### Example kubeconfig Secret

```bash
# Encode kubeconfig file
cat ~/.kube/config | base64

# Add as GitHub secret: KUBECONFIG_STAGING
```

### Deployment Manifests

Create deployment manifests in your repository:

```yaml
# k8s/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: app
        image: IMAGE_TAG
        ports:
        - containerPort: 3000
        env:
        - name: NODE_ENV
          value: "NAMESPACE"
```

## Best Practices

### Security

* Use minimal required Kubernetes RBAC permissions
* Store kubeconfig files as GitHub secrets (base64 encoded)
* Rotate credentials regularly
* Use namespaces for environment isolation

### Reliability

* Set appropriate resource requests and limits
* Configure liveness and readiness probes
* Use deployment strategies that minimize downtime
* Implement proper health checks

### Monitoring

* Monitor deployment progress and status
* Set up alerts for deployment failures
* Log deployment events and changes
* Track deployment metrics and performance

### Rollback Strategy

* Keep previous deployment versions available
* Test rollback procedures regularly
* Maintain deployment history
* Automate rollback on critical failures

## Troubleshooting

### Common Issues

#### Deployment Timeout

```yaml
# Increase timeout for large deployments
with:
  timeout: 600  # 10 minutes
```

#### Permission Denied

* Verify RBAC permissions in target cluster
* Check service account configuration
* Validate kubeconfig context

#### Image Pull Errors

* Verify image tag exists in registry
* Check registry credentials and permissions
* Ensure network connectivity to registry

#### Resource Constraints

* Check cluster resource availability
* Verify resource requests and limits
* Monitor cluster capacity

## Debugging Steps

1. Check Deployment Status

```bash
kubectl get deployments -n <namespace>
kubectl describe deployment <name> -n <namespace>
```

2. Check Pod Status

```bash
kubectl get pods -n <namespace>
kubectl logs <pod-name> -n <namespace>
```

3. Check Service Status

```bash
kubectl get services -n <namespace>
kubectl describe service <name> -n <namespace>
```

4. Check Events

```bash
kubectl get events -n <namespace>
```

## Advanced Usage

### Custom Health Checks

```yaml
- name: Deploy with Custom Health Check
  uses: your-username/github-actions-templates/.github/actions/deploy-environment@main
  with:
    environment: production
    image-tag: ${{ needs.build.outputs.image-tag }}
    kube-config: ${{ secrets.KUBECONFIG }}

- name: Custom Health Verification
  run: |
    # Wait for deployment to be ready
    kubectl wait --for=condition=available deployment/app --timeout=600s
    
    # Run custom health checks
    ./scripts/health-check.sh
```

### Multi-Region Deployment

```yaml
- name: Deploy to US-East
  uses: your-username/github-actions-templates/.github/actions/deploy-environment@main
  with:
    environment: production-us-east
    image-tag: ${{ needs.build.outputs.image-tag }}
    kube-config: ${{ secrets.KUBECONFIG_US_EAST }}

- name: Deploy to EU-West
  uses: your-username/github-actions-templates/.github/actions/deploy-environment@main
  with:
    environment: production-eu-west
    image-tag: ${{ needs.build.outputs.image-tag }}
    kube-config: ${{ secrets.KUBECONFIG_EU_WEST }}
```

## Contributing

To contribute to this action:
1. Test with different Kubernetes versions and configurations
2. Maintain compatibility with major cloud providers (EKS, GKE, AKS)
3. Add support for additional deployment strategies
4. Update documentation for new features

## License

This action is part of the GitHub Actions Templates repository and is licensed under the MIT License.