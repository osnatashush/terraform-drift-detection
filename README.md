# github-actions-terraforn-drift-detection

This repository provides a reusable GitHub Action for Terraform drift detection. It automatically checks for configuration drift in your Terraform-managed infrastructure and notifies your team via Email and GitHub Issues.

By using this action, you ensure that your Terraform state remains in sync with your configuration, preventing unexpected infrastructure changes.

## How it works

This GitHub Action follows these steps to perform Terraform Drift Detection:

1. Runs terraform plan to compare the desired state (code) with the actual state (deployed infrastructure).
2. Checks for drifts:

- If changes are detected, an issue is created or updated in the repository with the Terraform plan output.
- If no drifts are found, it closes any previously opened drift issue.

3. Handles failures:

- If the workflow itself fails (e.g., Terraform command errors, AWS credentials issues), an issue is created with details of the failure.

4. Sends notifications to Email for both drifts and failures, ensuring your team stays informed.

## Features

✔️ Automated Drift Detection: Ensures infrastructure matches the Terraform configuration.

✔️ GitHub Issue Management: Opens/updates/auto-closes issues based on drift status.

✔️ Email Notifications: Notifies the team about detected drifts and failures.

✔️ Secure AWS Access: Uses IAM role assumption for secure authentication.

✔️ Customizable Inputs: Supports different environments and AWS accounts.

## Inputs

- `working_directory`: The directory where Terraform files are located (default: `./`)
- `emails`: emails addresses for notifications (required)
- `environment`: The environment name (e.g., `dev`, `prod`) (required)
- `aws_region`: AWS Region to use for Terraform (required)
- `aws_role`: IAM role ARN to assume for AWS credentials (required)
- `aws_account_id`: AWS account ID (required)
- `github_token`: GitHub token for the running (required)

## Example Usage

```yaml
uses: delivops/github-actions-terraforn-drift-detection@v1.0.17
with:
  working_directory: "env/grafana"
  emails: ${{ secrets.EMAILS_LIST }}
  environment: "Grafana"
  aws_region: ${{ secrets.AWS_DEFAULT_REGION }}
  aws_role: "github_terraform"
  aws_account_id: ${{ secrets.AWS_ACCOUNT_ID }}
  github_token: ${{ secrets.GITHUB_TOKEN }}
```

## Behavior Scenarios

✅ No Drift Detected
Terraform finds no differences between the configuration and the actual infrastructure.
If a previous drift issue exists, it will be automatically closed.
No alert is sent to Email.

⚠️ Drift Detected
Terraform detects unexpected changes in the infrastructure.
An issue is created or updated in the repository with details of the drift (Terraform plan output).
A Email notification is sent to alert the team.

❌ Workflow Failure
If Terraform fails (e.g., misconfiguration, authentication error), an issue is created with error details.
A Email notification is sent indicating the failure.
