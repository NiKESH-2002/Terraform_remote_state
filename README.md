# Terraform S3 Backend

Creates an S3 bucket and DynamoDB table to use as a Terraform remote backend.

## What It Creates

| Resource | Name | Purpose |
|---|---|---|
| S3 Bucket | `<account-id>-terraform-states` | Stores the `terraform.tfstate` file |
| DynamoDB Table | `terraform-lock` | Handles state locking |

## Features

- S3 bucket versioning enabled — full revision history of state files
- AES256 server-side encryption on the S3 bucket
- DynamoDB state locking — prevents concurrent `terraform apply` runs

## Usage

**Step 1 — Deploy this module first:**
```bash
terraform init
terraform apply
```

**Step 2 — Copy the outputs into your project's backend config:**
```hcl
terraform {
  backend "s3" {
    bucket         = "<s3_bucket_name output>"
    key            = "some_environment/terraform.tfstate"
    region         = "<s3_bucket_region output>"
    encrypt        = true
    dynamodb_table = "<dynamodb_table_name output>"
  }
}
```

## Outputs

| Output | Description |
|---|---|
| `s3_bucket_name` | Name of the S3 bucket |
| `s3_bucket_arn` | ARN of the S3 bucket |
| `s3_bucket_region` | Region of the S3 bucket |
| `dynamodb_table_name` | Name of the DynamoDB table |
| `dynamodb_table_arn` | ARN of the DynamoDB table |

## Requirements

| Tool | Version |
|---|---|
| Terraform | >= 0.12 |
| AWS Provider | configured via environment or IAM role |

## Notes

- This module itself uses **local state** — it cannot use a remote backend that doesn't exist yet.
- Run this module once per AWS account. All projects in the account can share the same backend bucket using different `key` paths.
