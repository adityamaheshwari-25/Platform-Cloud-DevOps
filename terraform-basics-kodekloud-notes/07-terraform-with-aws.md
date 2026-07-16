# Module 7 — Terraform with AWS

## Why AWS appears in this course

AWS provides real cloud resources for practicing providers, authentication, IAM, S3, and DynamoDB. The Terraform workflow is provider-independent; the same ideas later apply to Azure’s `azurerm` provider.

## AWS setup essentials

- Create a normal AWS account and enable MFA on the root user.
- Do not use root credentials for everyday work.
- Use IAM roles, temporary credentials, or AWS IAM Identity Center where possible.
- Grant least-privilege permissions.
- Configure a budget before creating resources.
- Destroy lab resources when finished.

## AWS provider

```hcl
terraform {
  required_providers {
    aws = {
      source = "hashicorp/aws"
    }
  }
}

provider "aws" {
  region = "ap-south-1"
}
```

Initialize the provider:

```bash
terraform init
```

## Authentication

The AWS provider can use the standard AWS credential chain, including environment variables, shared AWS configuration, profiles, and instance/task roles.

Example AWS CLI configuration for a lab:

```bash
aws configure
aws sts get-caller-identity
```

Never place access keys directly in `.tf` files:

```hcl
# Do not do this
provider "aws" {
  access_key = "..."
  secret_key = "..."
}
```

Hard-coded credentials can leak through Git history, logs, plans, or state.

## IAM basics

| Item | Purpose |
|---|---|
| User | Identity for a person or legacy workload |
| Group | Collection of users |
| Role | Assumable identity with temporary credentials |
| Policy | JSON permissions document |

An IAM policy statement specifies an effect, actions, resources, and optional conditions.

```hcl
resource "aws_iam_user" "developer" {
  name = "terraform-lab-developer"
}

resource "aws_iam_policy" "read_buckets" {
  name = "terraform-lab-read-buckets"

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Effect   = "Allow"
      Action   = ["s3:ListAllMyBuckets"]
      Resource = "*"
    }]
  })
}
```

`jsonencode` is safer and easier than manually building JSON strings.

## Amazon S3 with Terraform

S3 stores objects inside buckets. Bucket names must be globally unique within the AWS partition.

```hcl
resource "aws_s3_bucket" "logs" {
  bucket = var.bucket_name

  tags = {
    Environment = "dev"
    ManagedBy   = "terraform"
  }
}

resource "aws_s3_bucket_versioning" "logs" {
  bucket = aws_s3_bucket.logs.id

  versioning_configuration {
    status = "Enabled"
  }
}
```

Production buckets normally also need encryption, public-access blocking, lifecycle policies, and carefully designed access policies.

## DynamoDB with Terraform

DynamoDB is AWS’s managed NoSQL key-value/document database.

```hcl
resource "aws_dynamodb_table" "locks" {
  name         = "terraform-lab-locks"
  billing_mode = "PAY_PER_REQUEST"
  hash_key     = "LockID"

  attribute {
    name = "LockID"
    type = "S"
  }
}
```

Older Terraform S3-backend configurations use DynamoDB for state locking. Current Terraform supports native S3 lockfiles; DynamoDB-based locking is deprecated in current Terraform documentation.

## Safe AWS workflow

```bash
aws sts get-caller-identity
terraform init
terraform fmt -check
terraform validate
terraform plan -out=tfplan
terraform apply tfplan
terraform destroy
```

## Common mistakes

- Hard-coding credentials in Terraform.
- Giving administrator access when a narrower policy is enough.
- Forgetting that S3 bucket names are globally unique.
- Creating billable resources and forgetting to destroy them.
- Manually changing resources without checking the next Terraform plan.

## Quick check

1. Should AWS keys be written in `.tf` files? **No.**
2. Which AWS command confirms the active identity? **`aws sts get-caller-identity`.**
3. What does `jsonencode` help create? **Valid JSON from Terraform values.**
4. Do AWS concepts make Terraform AWS-only? **No; providers let Terraform manage many platforms.**

## References

- [AWS provider documentation](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- [AWS provider authentication](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#authentication-and-configuration)
- [AWS IAM security best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)

