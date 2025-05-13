# Connect EKS

[![License](https://img.shields.io/github/license/didx-xyz/workflows.svg)](https://github.com/didx-xyz/workflows/blob/master/LICENSE)

A GitHub Action that simplifies connecting to an EKS cluster by setting up a Tailscale VPN connection, assuming an AWS
IAM role, and configuring kubectl access in a single step.

## Description

This action streamlines the process of establishing a secure connection to an Amazon EKS cluster by:

1. Connecting to a Tailscale VPN network
2. Assuming an AWS IAM role with the necessary permissions
3. Updating your kubeconfig file to connect to the specified EKS cluster

This is particularly useful for CI/CD workflows that need to interact with EKS clusters in private networks.

## Prerequisites

- AWS credentials with permissions to assume the specified role
- An EKS cluster running in AWS
- Either a Tailscale auth key or OAuth credentials for Tailscale authentication

## Inputs

| Input | Description | Required | Default |
| ----- | ----------- | -------- | ------- |
| `aws-region` | The AWS region where the EKS cluster is located | Yes | N/A |
| `aws-role-arn` | The ARN of the AWS role to assume | Yes | N/A |
| `aws-role-session-name` | The name of the AWS role session | Yes | N/A |
| `cluster-name` | The name of the EKS cluster | Yes | N/A |
| `tailscale-authkey` | The Tailscale auth key | No | `''` |
| `tailscale-oauth-client-id` | The Tailscale OAuth client ID | No | `''` |
| `tailscale-oauth-secret` | The Tailscale OAuth client secret | No | `''` |
| `tailscale-tags` | The Tailscale tags to use | No | `''` |

**Note**: You must provide either a `tailscale-authkey` OR both
`tailscale-oauth-client-id` and `tailscale-oauth-secret` for Tailscale
authentication.

## Usage

### Basic Example

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Connect to EKS
        uses: didx-xyz/workflows/connect-eks@master
        with:
          aws-region: us-west-2
          aws-role-arn: arn:aws:iam::123456789012:role/eks-admin
          aws-role-session-name: github-action
          cluster-name: my-eks-cluster
          tailscale-authkey: ${{ secrets.TAILSCALE_AUTHKEY }}

      - name: Deploy to Kubernetes
        run: |
          kubectl get nodes
          # Additional kubectl commands
```

### Using OAuth for Tailscale Authentication

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Connect to EKS
        uses: didx-xyz/workflows/connect-eks@v1
        with:
          aws-region: us-west-2
          aws-role-arn: arn:aws:iam::123456789012:role/eks-admin
          aws-role-session-name: github-action
          cluster-name: my-eks-cluster
          tailscale-oauth-client-id: ${{ secrets.TAILSCALE_OAUTH_CLIENT_ID }}
          tailscale-oauth-secret: ${{ secrets.TAILSCALE_OAUTH_SECRET }}
          tailscale-tags: tag:ci
```

## How It Works

This action performs the following steps:

1. **Connect to Tailscale**: Establishes a VPN connection using Tailscale to securely access private networks where your
EKS cluster may be located.

2. **Configure AWS Credentials**: Uses the AWS Actions credentials provider to assume the specified IAM role, which
should have permissions to interact with the EKS cluster.

3. **Update Kubeconfig**: Runs `aws eks update-kubeconfig` to configure kubectl with the necessary credentials and
endpoint information for your EKS cluster.

After these steps are completed, subsequent steps in your workflow can use `kubectl` to interact with your EKS cluster.

## Security Considerations

- Store sensitive information like Tailscale auth keys and OAuth credentials as GitHub Secrets.
- Use a dedicated AWS IAM role with least-privilege permissions.
- Consider using Tailscale tags to limit the network access of the VPN connection.

## Limitations

- This action requires a runner with support for Tailscale and AWS CLI.
- The connection is only valid for the duration of the workflow run.
- The AWS role session has a maximum duration determined by the role's settings.

## License

This project is licensed under the terms specified in the repository.

## Contributing

Contributions to improve the action are welcome.

Please visit the [didx-xyz/workflows](https://github.com/didx-xyz/workflows) repository for contribution guidelines.
