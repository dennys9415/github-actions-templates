# GitHub Actions Templates 🚀

A comprehensive collection of reusable, production-ready GitHub Actions workflows and composite actions to streamline your CI/CD pipelines.

## 📦 Structure

```
github-actions-templates/
├── .github/
│   ├── workflows/
│   │   ├── ci.yml                    # CI template
│   │   ├── cd.yml                    # CD template
│   │   ├── security-scan.yml         # Security scanning template
│   │   └── release.yml               # Release template
│   ├── actions/
│   │   ├── deploy-environment/       # Custom composite action
│   │   │   ├── action.yml
│   │   │   └── README.md
│   │   ├── setup-environment/        # Custom composite action
│   │   │   ├── action.yml
│   │   │   └── README.md
│   │   └── notify-team/              # Custom composite action
│   │       ├── action.yml
│   │       └── README.md
│   └── ISSUE_TEMPLATE/
│       ├── bug_report.md
│       └── feature_request.md
├── examples/
│   ├── nodejs-app/
│   │   └── .github/workflows/ci.yml
│   ├── python-app/
│   │   └── .github/workflows/ci.yml
│   └── docker-app/
│       └── .github/workflows/ci.yml
├── docs/
│   ├── getting-started.md
│   ├── best-practices.md
│   └── contributing.md
├── LICENSE
├── README.md
├── CODE_OF_CONDUCT.md
└── CONTRIBUTING.md
```

## 📦 Templates Included

### Workflow Templates
- **CI Pipeline** - Automated testing, building, and code quality checks
- **CD Pipeline** - Deployment to various environments
- **Security Scanning** - SAST, dependency scanning, and container security
- **Release Automation** - Version tagging, changelog generation, and publishing
- **Scheduled Maintenance** - Regular maintenance tasks

### Composite Actions
- **Setup Environment** - Consistent environment configuration
- **Notify Team** - Multi-platform notifications
- **Docker Build & Push** - Container image management
- **Deploy to Environment** - Environment-specific deployments

## 🚀 Quick Start

### Using Workflow Templates

1. **Copy a workflow to your repository:**
```bash
mkdir -p .github/workflows
curl -o .github/workflows/ci.yml \
https://raw.githubusercontent.com/your-username/github-actions-templates/main/.github/workflows/ci.yml
```

2. Customize the workflow:

* Update environment variables
* Modify build/test commands
* Configure your deployment targets

### Using Composite Actions

```yaml
- name: Setup Environment
  uses: your-username/github-actions-templates/.github/actions/setup-environment@main
  with:
    node-version: '18'
    python-version: '3.11'
```

## 📚 Examples

Check out the examples directory for language-specific implementations:
* Node.js Application
* Python Application
* Docker Application

## 🛠️ Usage Guide

### CI Pipeline

The CI template includes:
* Multi-version testing
* Code linting and formatting
* Security scanning
* Test coverage reporting
* Cache optimization

### CD Pipeline

The CD template supports:
* Environment-based deployments
* Manual approval gates
* Rollback capabilities
* Multi-platform deployments

### Security Scanning

* Dependency vulnerability scanning
* Static Application Security Testing (SAST)
* Container image scanning
* Secret detection

## 🤝 Contributing

We welcome contributions! Please see our Contributing Guide and Code of Conduct.

### Adding New Templates

1. Follow the existing naming conventions
2. Include comprehensive documentation
3. Add corresponding examples
4. Test thoroughly before submitting

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🙏 Acknowledgments

* GitHub Actions team for the amazing platform
* Community contributors
* All the action creators we build upon

