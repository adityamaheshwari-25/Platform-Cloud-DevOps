# Module 12 — Functions, Conditional Expressions, and Workspaces

## Built-in functions

Terraform functions transform and combine values. Function calls use:

```hcl
FUNCTION_NAME(argument_1, argument_2)
```

Examples:

```hcl
upper("dev")                         # "DEV"
length(["a", "b", "c"])           # 3
max(10, 20, 5)                      # 20
lookup({ dev = 1, prod = 3 }, "prod", 1)
merge({ a = 1 }, { b = 2 })
toset(["dev", "dev", "prod"])      # removes duplicates
```

## Useful function groups

| Group | Examples |
|---|---|
| String | `upper`, `lower`, `split`, `join`, `replace`, `format` |
| Numeric | `min`, `max`, `ceil`, `floor` |
| Collection | `length`, `merge`, `lookup`, `contains`, `flatten`, `distinct` |
| Type conversion | `tostring`, `tonumber`, `tolist`, `toset`, `tomap` |
| Filesystem | `file`, `templatefile`, `fileset` |
| Encoding | `jsonencode`, `jsondecode`, `base64encode` |
| Defensive evaluation | `try`, `can`, `coalesce` |

Terraform functions do not use the same syntax or behavior as functions from a general-purpose programming language. Check the official function documentation when uncertain.

## Terraform console

Use the interactive console to test expressions:

```bash
terraform console
```

Example session:

```text
> length(["dev", "test", "prod"])
3
> upper("terraform")
"TERRAFORM"
> 2 > 1 ? "yes" : "no"
"yes"
```

Exit with `exit` or Ctrl+D.

## Conditional expressions

Syntax:

```hcl
condition ? true_value : false_value
```

Example:

```hcl
instance_type = var.environment == "prod" ? "t3.medium" : "t3.micro"
```

Conditional creation with `count`:

```hcl
resource "local_file" "debug" {
  count    = var.enable_debug ? 1 : 0
  filename = "debug.txt"
  content  = "enabled"
}
```

The true and false result values must have compatible types so Terraform can determine the expression’s result type.

Avoid deeply nested conditions. Use locals, maps, or modules when they make the intent clearer.

## Local values

Locals assign a name to reusable expressions:

```hcl
locals {
  name_prefix = "${var.project}-${var.environment}"
  common_tags = {
    Project     = var.project
    Environment = var.environment
    ManagedBy   = "terraform"
  }
}
```

Reference them with:

```hcl
local.name_prefix
local.common_tags
```

Locals reduce repetition but are not user inputs.

## Terraform CLI workspaces

CLI workspaces allow one configuration and backend to maintain multiple named state instances.

Commands:

```bash
terraform workspace list
terraform workspace show
terraform workspace new dev
terraform workspace select dev
terraform workspace select -or-create test
terraform workspace delete test
```

Current workspace name:

```hcl
terraform.workspace
```

Example:

```hcl
locals {
  instance_sizes = {
    default = "t3.micro"
    dev     = "t3.micro"
    prod    = "t3.medium"
  }

  instance_type = lookup(
    local.instance_sizes,
    terraform.workspace,
    local.instance_sizes.default
  )
}
```

## When to use workspaces

Useful when:

- Environments use nearly identical configuration.
- The same team and access boundary manage them.
- You need temporary or parallel state instances.

Avoid relying on CLI workspaces when environments require:

- Different credentials or access controls
- Different backends or isolation boundaries
- Substantially different architecture
- Independent lifecycle and release processes

For strong production separation, separate root configurations, backend keys, accounts/subscriptions, or repositories are often clearer.

## Workspace cautions

- Every new working directory starts with the `default` workspace.
- Commands operate against the currently selected workspace.
- Always run `terraform workspace show` before a sensitive apply or destroy.
- Workspaces isolate state; they do not automatically isolate credentials, code, or cloud accounts.
- A workspace is not the same thing as an HCP Terraform workspace, which also represents configuration, variables, state, runs, and permissions.

## Quick check

1. Which command lets you test expressions interactively? **`terraform console`.**
2. What is the conditional syntax? **`condition ? true_value : false_value`.**
3. What do CLI workspaces separate? **State instances.**
4. Do workspaces automatically create security isolation? **No.**

## References

- [Terraform built-in functions](https://developer.hashicorp.com/terraform/language/functions)
- [Conditional expressions](https://developer.hashicorp.com/terraform/language/expressions/conditionals)
- [Local values](https://developer.hashicorp.com/terraform/language/values/locals)
- [Terraform workspaces](https://developer.hashicorp.com/terraform/language/state/workspaces)

