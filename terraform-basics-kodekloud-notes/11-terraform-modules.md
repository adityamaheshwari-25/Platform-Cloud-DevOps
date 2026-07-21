# Module 11 — Terraform Modules

# **terraform modules used for provising different environments using the same infra**

Instead of copying and pasting code, you write the infrastructure logic once inside a **reusable child module**. You then call that same module across your different environments, altering its behavior by passing in environment-specific **input variables** (such as distinct instance sizes, cluster counts, or database names).

## Reusable module structure for multiple environments

```text
├── modules/
│   └── web_app/               # The single blueprint/source of truth
│       ├── main.tf
│       ├── variables.tf
│       └── outputs.tf
│
└── environments/              # Where the environments live separately
    ├── dev/
    │   ├── main.tf            # Calls ../../modules/web_app with dev variables
    │   └── backend.tf         # Isolated dev.tfstate file
    ├── staging/
    │   ├── main.tf            # Calls ../../modules/web_app with staging variables
    │   └── backend.tf         # Isolated staging.tfstate file
    └── prod/
        ├── main.tf            # Calls ../../modules/web_app with prod variables
        └── backend.tf         # Isolated prod.tfstate file
```

### Reusing the module

`environments/dev/main.tf` — lightweight development settings:

```hcl
module "my_app" {
  source         = "../../modules/web_app"
  environment    = "development"
  instance_type  = "t3.micro"
  instance_count = 1
}
```

`environments/prod/main.tf` — the same module with production settings:

```hcl
module "my_app" {
  source         = "../../modules/web_app"
  environment    = "production"
  instance_type  = "m5.large"
  instance_count = 3
}
```

## What is a module?

A module is a collection of Terraform files in one directory.

**All `.tf` files in the working directory are part of the Terraform configuration, and Terraform automatically loads them together.**

- The directory where you run Terraform is the **root module**.
- A module called by another module is a **child module**.
- Modules package related resources behind a reusable interface.

## Why use modules?

- Reuse proven infrastructure patterns.
- Standardize security and naming.
- Reduce repeated configuration.
- Hide unnecessary implementation detail.
- Test and version infrastructure components independently.

A good module is cohesive: it solves one clear infrastructure problem without exposing every internal detail.

## Typical module structure

```text
modules/network/
├── main.tf
├── variables.tf
├── outputs.tf
├── versions.tf
└── README.md
```

These names are conventions, not special execution files. Terraform loads the module’s `.tf` files together.

## Creating a local module

Child module:

```hcl
# modules/file/main.tf
resource "local_file" "this" {
  filename = var.filename
  content  = var.content
}
```

```hcl
# modules/file/variables.tf
variable "filename" {
  type = string
}

variable "content" {
  type = string
}
```

```hcl
# modules/file/outputs.tf
output "id" {
  value = local_file.this.id
}
```

Call it from the root module:

```hcl
module "welcome_file" {
  source = "./modules/file"

  filename = "welcome.txt"
  content  = "Hello"
}
```

Read its output with:

```hcl
module.welcome_file.id
```

Run `terraform init` after adding or changing module sources.

## Module inputs and outputs

| Interface | Implemented with | Direction |
|---|---|---|
| Input | `variable` block | Parent → child |
| Output | `output` block | Child → parent |

Only expose inputs that callers genuinely need. Add types, descriptions, validations, and sensible defaults.

## Registry modules

Example public registry source:

```hcl
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "~> 6.0"

  name = "dev-vpc"
  cidr = "10.0.0.0/16"
}
```

Before using a third-party module:

- Review its source code.
- Check maintainership and recent releases.
- Read upgrade notes.
- Pin an appropriate version.
- Understand the resources and permissions it creates.
- Test upgrades in a nonproduction environment.

Local-path modules do not use the `version` argument. Registry and supported remote sources can be versioned according to their source type.

## Providers in modules

Reusable child modules should declare provider requirements but normally receive provider configurations from the parent.

For aliased configurations, pass providers explicitly:

```hcl
module "regional_app" {
  source = "./modules/app"

  providers = {
    aws = aws.secondary
  }
}
```

Avoid hard-coding credentials or environment-specific provider settings inside reusable modules.

## Multiple module instances

Modules can use `for_each` or `count`:

```hcl
module "environment" {
  for_each = toset(["dev", "test"])
  source   = "./modules/environment"

  name = each.key
}
```

An instance address would be:

```text
module.environment["dev"]
```

## Module design checklist

- Clear purpose and README
- Typed and documented inputs
- Useful outputs only
- No embedded credentials
- Sensible version constraints
- Consistent naming and tags
- Examples and automated validation/tests
- Minimal provider-specific assumptions beyond the module’s purpose

## Quick check

1. What is the directory where Terraform runs called? **The root module.**
2. How does a parent pass values into a child? **Module arguments mapped to child input variables.**
3. How does a child expose a value? **An output value.**
4. Should third-party module versions be left completely unpinned? **No.**

## References

- [Terraform modules](https://developer.hashicorp.com/terraform/language/modules)
- [Module syntax](https://developer.hashicorp.com/terraform/language/modules/syntax)
- [Developing modules](https://developer.hashicorp.com/terraform/language/modules/develop)
- [Terraform Registry modules](https://registry.terraform.io/browse/modules)
