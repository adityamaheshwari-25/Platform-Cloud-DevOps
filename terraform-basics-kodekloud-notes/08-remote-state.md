# Module 8 — Remote State

## Why remote state?

Local state is difficult to share and protect in a team. A remote backend stores state in a shared system.

Benefits include:

- Centralized state access
- Better backup and recovery options
- Encryption and access control
- Safer collaboration
- State locking when supported
- Reduced need to copy state files between machines

## Backend concept

A backend determines where Terraform stores state and, depending on the backend, how operations and locking work.

Backend configuration belongs inside the `terraform` block:

```hcl
terraform {
  backend "s3" {
    bucket       = "company-terraform-state"
    key          = "dev/network/terraform.tfstate"
    region       = "ap-south-1"
    encrypt      = true
    use_lockfile = true
  }
}
```

Important:

- The backend bucket normally must exist before initialization.
- Backend blocks cannot use normal input variables or resource references.
- The state `key` should clearly identify the project and environment.
- Enable bucket versioning for recovery.
- Block public access and restrict IAM permissions.

## State locking

Locking prevents two writers from changing the same state simultaneously. Without locking, concurrent applies can corrupt state or create conflicting infrastructure changes.

Current Terraform supports native S3 lockfiles with:

```hcl
use_lockfile = true
```

The older S3-backend method using DynamoDB locking is deprecated. You may still encounter it in existing configurations and older course material.

## Migrating to a backend

After adding or changing backend configuration:

```bash
terraform init -migrate-state
```

Terraform asks to copy the existing state to the new backend. Back up critical state and verify the destination before migrating.

For a backend reconfiguration that does not migrate existing state:

```bash
terraform init -reconfigure
```

Do not use `-reconfigure` as a casual substitute for understanding where the existing state lives.

## State commands

| Command | Purpose |
|---|---|
| `terraform state list` | List managed resource addresses |
| `terraform state show ADDRESS` | Show one managed object |
| `terraform state mv SOURCE DESTINATION` | Move/rename a state binding |
| `terraform state rm ADDRESS` | Stop managing an object without deleting it |
| `terraform state pull` | Print remote/local state to stdout |
| `terraform force-unlock LOCK_ID` | Remove a lock after confirming no operation is active |

Examples:

```bash
terraform state list
terraform state show 'aws_s3_bucket.logs'
terraform state mv 'aws_instance.web' 'aws_instance.app'
```

State commands modify Terraform’s records, not necessarily the real object. Use them carefully and prefer configuration-driven `moved` blocks for refactoring when appropriate.

## `moved` block

For a reviewed, repeatable resource rename:

```hcl
moved {
  from = aws_instance.web
  to   = aws_instance.app
}
```

This tells Terraform that an existing object has a new address instead of asking it to destroy and recreate the resource.

## Force-unlock warning

Only run `terraform force-unlock` after verifying that no valid Terraform operation still holds the lock. Unlocking an active operation can allow concurrent writers and damage state.

## Remote state security checklist

- Least-privilege access
- Encryption at rest and in transit
- Versioning and recovery plan
- State locking
- Audit logging
- Separate state by environment or trust boundary
- No secrets in backend configuration files

## Quick check

1. Why use state locking? **To prevent concurrent state writers.**
2. Which command migrates existing state to a new backend? **`terraform init -migrate-state`.**
3. Does `terraform state rm` delete the cloud resource? **No; it removes the binding from state.**
4. What is the current native S3 locking option? **`use_lockfile = true`.**

## References

- [Terraform backends](https://developer.hashicorp.com/terraform/language/backend)
- [S3 backend](https://developer.hashicorp.com/terraform/language/backend/s3)
- [Terraform state commands](https://developer.hashicorp.com/terraform/cli/commands/state)
- [`moved` blocks](https://developer.hashicorp.com/terraform/language/moved)

