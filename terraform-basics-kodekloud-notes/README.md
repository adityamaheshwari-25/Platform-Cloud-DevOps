# Terraform Basics — KodeKloud Module Notes

Concise, revision-friendly notes organized according to the current KodeKloud **Terraform Basics Training Course**.

## Modules

1. [Introduction](01-introduction.md)
2. [Introduction to Infrastructure as Code](02-introduction-to-infrastructure-as-code.md)
3. [Getting Started with Terraform](03-getting-started-with-terraform.md)
4. [Terraform Basics](04-terraform-basics.md)
5. [Terraform State](05-terraform-state.md)
6. [Working with Terraform](06-working-with-terraform.md)
7. [Terraform with AWS](07-terraform-with-aws.md)
8. [Remote State](08-remote-state.md)
9. [Terraform Provisioners](09-terraform-provisioners.md)
10. [Import, Tainting, and Debugging](10-import-taint-and-debugging.md)
11. [Terraform Modules](11-terraform-modules.md)
12. [Functions, Conditional Expressions, and Workspaces](12-functions-conditionals-and-workspaces.md)

## Recommended study method

1. Read one module note.
2. Type the examples yourself instead of copying them blindly.
3. Complete the corresponding KodeKloud labs.
4. Run `terraform plan` and predict the result before reading it.
5. Destroy lab resources when finished, especially AWS resources.

## Core Terraform workflow

```text
Write configuration → terraform init → terraform fmt/validate
                    → terraform plan → terraform apply
                    → modify or terraform destroy
```

## Important for an Azure DevOps path

The course uses AWS for its cloud labs, but the important Terraform concepts—providers, variables, state, modules, lifecycle, and remote backends—transfer directly to Azure. After this course, continue with **Terraform on Azure** and practice the `azurerm` provider.

## References

- [KodeKloud — Terraform Basics Training Course](https://learn.kodekloud.com/user/courses/terraform-basics-training-course)
- [Terraform documentation](https://developer.hashicorp.com/terraform/docs)
- [Terraform language documentation](https://developer.hashicorp.com/terraform/language)

