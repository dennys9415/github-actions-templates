# Contributing to GitHub Actions Templates

We love your input! We want to make contributing to this project as easy and transparent as possible, whether it's:

- Reporting a bug
- Discussing the current state of the code
- Submitting a fix
- Proposing new features
- Becoming a maintainer

## We Develop with Github

We use GitHub to host code, to track issues and feature requests, as well as accept pull requests.

## We Use [Github Flow](https://guides.github.com/introduction/flow/index.html)

Pull requests are the best way to propose changes to the codebase. We actively welcome your pull requests:

1. Fork the repo and create your branch from `main`.
2. If you've added code that should be tested, add tests.
3. If you've changed APIs, update the documentation.
4. Ensure the test suite passes.
5. Make sure your code lints.
6. Issue that pull request!

## Any contributions you make will be under the MIT Software License

When you submit code changes, your submissions are understood to be under the same [MIT License](LICENSE) that covers the project.

## Report bugs using Github's [issues](https://github.com/your-username/github-actions-templates/issues)

We use GitHub issues to track public bugs. Report a bug by [opening a new issue](); it's that easy!

## Write bug reports with detail, background, and sample code

**Great Bug Reports** tend to have:

- A quick summary and/or background
- Steps to reproduce
  - Be specific!
  - Give sample code if you can
- What you expected would happen
- What actually happens
- Notes (possibly including why you think this might be happening, or stuff you tried that didn't work)

## Template Contribution Guidelines

### Adding New Workflow Templates

1. **Follow Naming Conventions**
   - Use kebab-case for file names
   - Include descriptive names
   - Group related workflows

2. **Include Documentation**
   - Add comments in YAML files
   - Update README.md
   - Create examples if applicable

3. **Test Thoroughly**
   - Test with different configurations
   - Verify on multiple platforms
   - Check error handling

### Adding Composite Actions

1. **Clear Interface**
   - Define all inputs and outputs
   - Provide sensible defaults
   - Include usage examples

2. **Error Handling**
   - Handle common failure cases
   - Provide meaningful error messages
   - Use appropriate exit codes

3. **Performance**
   - Implement caching where appropriate
   - Minimize external dependencies
   - Optimize for common use cases

## License

By contributing, you agree that your contributions will be licensed under its MIT License.