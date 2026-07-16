# Module 10 — Import, Tainting, and Debugging

## Replacing a resource

Sometimes a managed object must be recreated even though its configuration has not changed.

Modern preferred workflow:

```bash
terraform plan -replace='aws_instance.web'
terraform apply -replace='aws_instance.web'
```

For safer automation, save and review the plan:

```bash
terraform plan -replace='aws_instance.web' -out=tfplan
terraform apply tfplan
```

## `terraform taint`

Older workflows mark an object as damaged:

```bash
terraform taint 'aws_instance.web'
terraform untaint 'aws_instance.web'
```

`terraform taint` is deprecated because it modifies state immediately and creates a separate step between review and apply. Prefer `-replace`, which makes the replacement request visible in the plan.

## Debugging Terraform

Enable Terraform logs with `TF_LOG`:

```bash
export TF_LOG=DEBUG
terraform plan
unset TF_LOG
```

Common levels include `TRACE`, `DEBUG`, `INFO`, `WARN`, and `ERROR`.

Write logs to a file:

```bash
export TF_LOG=TRACE
export TF_LOG_PATH=terraform-debug.log
terraform plan
unset TF_LOG TF_LOG_PATH
```

Logs may contain sensitive information. Protect them and remove them after troubleshooting.

## Troubleshooting sequence

1. Run `terraform fmt -check`.
2. Run `terraform validate`.
3. Read the full error, including the resource address and source line.
4. Confirm credentials and active account/subscription.
5. Confirm provider and Terraform versions.
6. Inspect `terraform plan`.
7. Check platform API errors and service limits.
8. Enable `TF_LOG` only when normal output is insufficient.

Useful commands:

```bash
terraform version
terraform providers
terraform state list
terraform state show 'RESOURCE_ADDRESS'
terraform show
```

## Importing existing infrastructure

Import connects an existing object to a Terraform resource address. Import does not magically understand your desired configuration; the matching resource configuration must exist or be generated and reviewed.

### CLI import workflow

First write a resource block:

```hcl
resource "aws_s3_bucket" "existing" {
  bucket = "existing-company-bucket"
}
```

Then import using the provider-specific ID:

```bash
terraform import aws_s3_bucket.existing existing-company-bucket
terraform plan
```

The import ID format differs by resource. Always check that resource’s provider documentation.

### Configuration-driven import

Modern Terraform also supports import blocks:

```hcl
import {
  to = aws_s3_bucket.existing
  id = "existing-company-bucket"
}
```

Then run:

```bash
terraform plan
terraform apply
```

For supported workflows, Terraform can generate an initial resource configuration:

```bash
terraform plan -generate-config-out=generated.tf
```

Generated configuration must be reviewed and simplified before production use.

## Import cautions

- Import changes state; it does not create the remote object.
- One remote object should map to only one Terraform resource address.
- After import, run a plan and reconcile every unexpected difference.
- Missing arguments may cause Terraform to update or replace the imported object.
- Back up state before risky state or import operations.

## Quick check

1. What is preferred over `terraform taint`? **`terraform apply -replace=ADDRESS` or a reviewed saved plan.**
2. Does import create the cloud resource? **No.**
3. What does import connect? **An existing object to a Terraform resource address in state.**
4. Can debug logs expose secrets? **Yes.**

## References

- [`terraform apply -replace`](https://developer.hashicorp.com/terraform/cli/commands/apply#replace-address)
- [`terraform taint`](https://developer.hashicorp.com/terraform/cli/commands/taint)
- [Terraform debugging](https://developer.hashicorp.com/terraform/internals/debugging)
- [Import existing resources](https://developer.hashicorp.com/terraform/language/import)

