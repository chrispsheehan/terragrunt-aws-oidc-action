# Execute Terraform & Terragrunt with AWS OIDC

This GitHub Action sets up **Terraform** and **Terragrunt**, authenticates to AWS via **OIDC**, and runs a specified `terragrunt` action (`apply` or `destroy`) with optional override variables.

---

## 🚀 Features

- Installs your specified versions of Terraform and Terragrunt
- Configures AWS credentials using OIDC
- Supports passing variables via JSON
- Automatically captures and returns Terraform outputs
- Sets `TF_IN_AUTOMATION=true` for CI-safe output and logging

---

## 📥 Inputs

| Name               | Description                                                   | Required | Default      |
|--------------------|---------------------------------------------------------------|----------|--------------|
| `tf_version`       | Version of Terraform to install                                | ❌        | `1.10.5`     |
| `tg_version`       | Version of Terragrunt to install                               | ❌        | `0.72.6`     |
| `aws_region`       | AWS region to use                                              | ❌        | `eu-west-2`  |
| `override_tg_vars` | Terragrunt variables in JSON format                            | ❌        | `{}`         |
| `aws_oidc_role_arn`| ARN of the IAM role to assume via OIDC                         | ✅        | —            |
| `tg_directory`     | Path to the directory containing your Terragrunt configuration | ✅        | —            |
| `tg_action`        | Terragrunt action to perform (`apply` or `destroy`)            | ✅        | `apply`      |

---

## 📤 Outputs

| Name         | Description                              |
|--------------|------------------------------------------|
| `tg_outputs` | All Terraform outputs in JSON format     |

---

## 🛠 Example Usage

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: read

    steps:
      - uses: actions/checkout@v4

      - name: Deploy Infrastructure
        uses: your-org/your-action-repo@main
        with:
          tf_version: "1.10.5"
          tg_version: "0.72.6"
          aws_oidc_role_arn: arn:aws:iam::123456789012:role/github-oidc-role
          tg_directory: ./infra/dev
          tg_action: apply
          override_tg_vars: '{"env": "dev", "region": "eu-west-2"}'

