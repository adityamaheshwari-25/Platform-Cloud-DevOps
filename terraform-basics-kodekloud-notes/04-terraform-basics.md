# Module 4 — Terraform Basics

## Providers

Providers are plugins that let Terraform communicate with external APIs.

```hcl
terraform {
  required_providers {
    local = {
      source  = "hashicorp/local"
      version = "~> 2.5"
    }
  }
}

provider "local" {}
```

- `required_providers` declares the provider source and version constraint.
- A `provider` block configures a provider, such as its region or endpoint.
- `terraform init` downloads the selected provider version.
- `.terraform.lock.hcl` records provider selections and checksums and should normally be committed to Git.

## Configuration directory

Terraform treats all `.tf` files in one working directory as a single module. Filenames help humans organize the configuration; they do not control execution order.

A common layout is:

```text
main.tf       # Resources and data sources
providers.tf  # Provider requirements/configuration
variables.tf  # Input variables
outputs.tf    # Output values
versions.tf   # Terraform/provider version constraints
terraform.tfvars
```

References create the dependency graph, not the order of files.

## Multiple provider configurations

Provider aliases allow multiple configurations of the same provider:

```hcl
provider "aws" {
  region = "ap-south-1"
}

provider "aws" {
  alias  = "secondary"
  region = "us-east-1"
}

resource "aws_s3_bucket" "secondary_logs" {
  provider = aws.secondary
  bucket   = "example-secondary-logs"
}
```

An unaliased provider is the default. A resource selects an alias with the `provider` meta-argument.

## Input variables

Variables make a configuration reusable.

```hcl
variable "environment" {
  description = "Deployment environment"
  type        = string
  default     = "dev"

  validation {
    condition     = contains(["dev", "test", "prod"], var.environment)
    error_message = "Environment must be dev, test, or prod."
  }
}
```

Reference it with:

```hcl
var.environment
```

### Useful variable arguments

| Argument | Purpose |
|---|---|
| `type` | Restricts the accepted data type |
| `default` | Makes the variable optional |
| `description` | Documents its meaning |
| `sensitive` | Redacts the value from normal CLI output |
| `nullable` | Controls whether `null` is valid |
| `validation` | Adds a custom acceptance rule |

### Supplying values

```bash
terraform apply -var='environment=prod'
terraform apply -var-file='prod.tfvars'
```

Environment variable:

```bash
export TF_VAR_environment=prod
```

Terraform also automatically loads `terraform.tfvars` and files ending in `.auto.tfvars`. Never commit secret `.tfvars` files.

## Collection variables

```hcl
variable "ports" {
  type    = list(number)
  default = [80, 443]
}

variable "tags" {
  type = map(string)
  default = {
    Environment = "dev"
    ManagedBy   = "terraform"
  }
}
```

## Resource attributes and references

A resource exports attributes after Terraform reads or creates it:

```hcl
resource "local_file" "message" {
  filename = "message.txt"
  content  = "hello"
}

resource "local_file" "copy" {
  filename = "copy.txt"
  content  = local_file.message.content
}
```

`local_file.message.content` is a resource-attribute reference.

## Dependencies

### Implicit dependency

Terraform detects a dependency from an expression such as:

```hcl
content = local_file.message.content
```

### Explicit dependency

Use `depends_on` only when a real dependency cannot be expressed through data references:

```hcl
resource "local_file" "marker" {
  filename   = "marker.txt"
  content    = "ready"
  depends_on = [local_file.message]
}
```

Prefer implicit dependencies because they let Terraform build a more accurate graph.

## Output values

Outputs expose useful values from a module:

```hcl
output "message_file" {
  description = "Path of the generated file"
  value       = local_file.message.filename
}
```

Read outputs with:

```bash
terraform output
terraform output message_file
terraform output -json
```

Mark confidential output as sensitive:

```hcl
output "token" {
  value     = var.api_token
  sensitive = true
}
```

`sensitive = true` hides normal display but does not encrypt the value in state.

## Quick check

1. What downloads provider plugins? **`terraform init`.**
2. Do `.tf` filenames define resource order? **No; references form the dependency graph.**
3. When should `depends_on` be used? **For a hidden dependency that cannot be expressed by a value reference.**
4. Does `sensitive = true` encrypt state? **No.**

## References

- [Provider requirements](https://developer.hashicorp.com/terraform/language/providers/requirements)
- [Input variables](https://developer.hashicorp.com/terraform/language/values/variables)
- [Resource dependencies](https://developer.hashicorp.com/terraform/language/meta-arguments/depends_on)
- [Output values](https://developer.hashicorp.com/terraform/language/values/outputs)

