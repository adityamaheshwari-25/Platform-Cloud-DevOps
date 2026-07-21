# Module 5 — Terraform State

Configuration = what you want

Real infrastructure = what actually exists

State = Terraform's memory connecting the two

## What state is

Terraform state is Terraform’s record of the infrastructure it manages. By default, local state is stored in:

```text
terraform.tfstate
```

State maps a Terraform resource address such as:

```text
aws_instance.web
```

to the corresponding real-world object, such as an EC2 instance ID.

## Why Terraform needs state

- Maps configuration addresses to provider objects.
- Stores resource attributes required by other resources.
- Tracks metadata and dependencies.
- Helps calculate create, update, replace, and delete actions.
- Improves performance by retaining known information.

Terraform does not manage resources by configuration alone; state is a core part of the workflow.

## The three views Terraform compares

| View | Meaning |
|---|---|
| Configuration | Desired infrastructure in `.tf` files |
| State | Terraform’s last recorded mapping and attributes |
| Real infrastructure | Current objects returned by provider APIs |

Differences between configuration and reality are called drift. A plan refreshes relevant information and proposes how to reach the configured desired state.

## Resource vs data source

| Resource | Data source |
|---|---|
| Keyword: `resource` | Keyword: `data` |
| Creates, updates, and destroys infrastructure. | Only reads existing infrastructure. |
| Also called a managed resource. | Also called a data resource. |

## State safety

State can contain:

- Resource IDs
- IP addresses and hostnames
- Configuration values
- Secrets returned by providers
- Values marked sensitive

Therefore:

- Do not commit `terraform.tfstate` to Git.
- Restrict access to state storage.
- Encrypt it at rest and in transit.
- Back it up and enable versioning where supported.
- Use remote state for team environments.
- Ensure only one writer modifies a state at a time.

Suggested `.gitignore` entries:

```gitignore
.terraform/
*.tfstate
*.tfstate.*
crash.log
crash.*.log
*.tfplan
*.tfvars
*.tfvars.json
```

Do not ignore `.terraform.lock.hcl`; it should normally be version-controlled.

## State is not a general database

Do not manually edit the JSON state file. Use Terraform commands such as `terraform state mv` or `terraform state rm` when a state operation is genuinely required.

State manipulation can make Terraform forget or misidentify infrastructure, so back up state and review the command carefully.

## Refresh behavior

Modern `terraform plan` and `terraform apply` refresh provider data as part of their normal work. To update state without changing remote objects:

```bash
terraform apply -refresh-only
```

Review the refresh-only plan before approving it.

## Local state limitations

Local state is acceptable for initial labs but weak for team use because it:

- Lives on one machine.
- Is easy to lose.
- Is difficult to share safely.
- Does not automatically coordinate concurrent writers.

Remote backends address these problems and are covered later.

## Quick check

1. What does state map? **Terraform resource addresses to real objects.**
2. Can sensitive values still exist in state? **Yes.**
3. Should `terraform.tfstate` be committed to Git? **No.**
4. Should `.terraform.lock.hcl` usually be committed? **Yes.**

## References

- [Purpose of Terraform state](https://developer.hashicorp.com/terraform/language/state/purpose)
- [Sensitive data in state](https://developer.hashicorp.com/terraform/language/state/sensitive-data)
- [Manage resource drift](https://developer.hashicorp.com/terraform/tutorials/state/resource-drift)
