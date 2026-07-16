# Module 3 — Getting Started with Terraform

## Installing Terraform

Install Terraform using HashiCorp’s official package instructions or a supported package manager, then verify it:

```bash
terraform version
terraform -help
```

Terraform is distributed as a CLI binary. Provider plugins are downloaded later by `terraform init`.

## HCL basics

Terraform configuration normally uses HashiCorp Configuration Language in `.tf` files.

```hcl
resource "local_file" "message" {
  filename = "message.txt"
  content  = "Hello from Terraform"
}
```

An HCL block contains:

```hcl
BLOCK_TYPE "LABEL_1" "LABEL_2" {
  argument = expression
}
```

For a resource:

- `resource` is the block type.
- `local_file` is the resource type.
- `message` is the local resource name.
- `filename` and `content` are arguments.

The resource address is:

```text
local_file.message
```

## Common value types

```hcl
name    = "web"
enabled = true
count   = 3
ports   = [80, 443]
labels  = {
  environment = "dev"
  owner       = "platform"
}
```

Terraform supports strings, numbers, booleans, lists/tuples, maps/objects, sets, and `null`.

## Comments

```hcl
# Single-line comment
// Single-line comment

/*
Multi-line comment
*/
```

## First workflow

```bash
terraform init
terraform fmt
terraform validate
terraform plan
terraform apply
```

| Command | Purpose |
|---|---|
| `terraform init` | Initialize the directory and download providers/modules |
| `terraform fmt` | Format Terraform files consistently |
| `terraform validate` | Check syntax and internal consistency |
| `terraform plan` | Preview proposed changes |
| `terraform apply` | Execute an approved plan |

## Updating infrastructure

Change the configuration and run:

```bash
terraform plan
terraform apply
```

Terraform compares configuration, state, and refreshed provider data to decide whether a resource is updated in place or replaced.

## Destroying infrastructure

Preview a destroy plan:

```bash
terraform plan -destroy
```

Destroy everything managed by the current configuration and state:

```bash
terraform destroy
```

Always read the plan carefully. Destroy is intentionally destructive.

## Saved plans

For a reviewed plan/apply workflow:

```bash
terraform plan -out=tfplan
terraform apply tfplan
```

Do not keep old saved-plan files indefinitely; they may contain sensitive values and can become stale.

## Quick lab

1. Create a `local_file` resource.
2. Run `init`, `fmt`, `validate`, and `plan`.
3. Apply it and inspect the created file.
4. Change its content and apply again.
5. Run `terraform destroy`.

## Exam and interview traps

- `init` downloads providers; `apply` creates or changes infrastructure.
- Terraform reads all `.tf` files in the working directory as one configuration.
- A resource’s local name is not necessarily the real cloud resource name.
- `validate` checks configuration validity; it does not guarantee the deployment will succeed.

## References

- [Install Terraform](https://developer.hashicorp.com/terraform/install)
- [Terraform configuration language](https://developer.hashicorp.com/terraform/language/syntax/configuration)
- [Core Terraform workflow](https://developer.hashicorp.com/terraform/intro/core-workflow)

