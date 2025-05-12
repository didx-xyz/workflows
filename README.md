# DIDx Reusable GitHub Workflows

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A collection of reusable GitHub workflows for the DIDx Organization focused on keeping things DRY (Don't Repeat Yourself).

## Overview

This repository contains a set of standardized GitHub Actions workflows that can be reused across multiple DIDx
repositories. By centralizing common CI/CD processes, we ensure consistency, reduce duplication, and simplify maintenance
across our projects.

## Why Reusable Workflows?

- **Consistency**: Enforce standardized processes across all repositories
- **Maintainability**: Update workflows in a single place rather than in each repository
- **Efficiency**: Reduce duplication and save time setting up new repositories
- **Best Practices**: Implement organizational best practices by default

## Available Workflows

Below is a list of the reusable workflows available in this repository:

### CI Workflows

- **build-and-test**: Standard build and test workflow for various languages and frameworks
- **code-quality**: Static code analysis and linting workflows
- **security-scan**: Security vulnerability scanning for code and dependencies

### CD Workflows

- **docker-build-push**: Build and push Docker images to container registries
- **deployment**: Deployment workflows for various environments (dev, staging, production)
- **release**: Create and publish releases with automatic versioning

## How to Use

To use these workflows in your repository, reference them in your workflow YAML files using the `uses` keyword with the
workflow path.

Example:

```yaml
name: CI

on:
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]

jobs:
  test:
    name: Run tests on EKS
    runs-on: ubuntu-latest
    steps:
      - name: Connect to EKS
        uses: didx-xyz/workflows/connect-eks@master
        with:
          aws-region: af-south-1
          aws-role-arn: arn:aws:iam::123456789012:role/eks-connect
          aws-role-session-name: github
          cluster-name: my-eks-cluster
          tailscale-authkey: ${{ secrets.TAILSCALE_AUTHKEY }}
```

### Workflow Inputs

Each workflow accepts specific inputs to customize behavior. Refer to individual workflow files for detailed
documentation on available inputs and their usage.

## Contributing

We welcome contributions to improve these workflows! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-workflow`)
3. Commit your changes (`git commit -m ':construction_worker: Add some amazing workflow'`)
4. Push to the branch (`git push origin feature/amazing-workflow`)
5. Open a Pull Request

### Development Guidelines

- Add proper documentation and examples for any new workflow
- Test workflows thoroughly before submitting
- Follow the established patterns for consistency
- Include appropriate input validation

## Related Projects

- [DIDx ACA-Py Cloud](https://github.com/didx-xyz/acapy-cloud): Cloud based deployment for Self-Sovereign Identity (SSI) applications

## About DIDx

DIDx is dedicated to building the future of Self-Sovereign Identity (SSI). We focus on creating tools and infrastructure
for decentralized identity applications and services.

## License

This project is licensed under the MIT License - see the [LICENSE](./LICENSE) file for details.
