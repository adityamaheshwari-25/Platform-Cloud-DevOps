# Module 1 — Introduction

## Course purpose

This beginner course teaches how to define and manage infrastructure using Terraform configuration instead of manually creating every resource.

The course moves through:

- Infrastructure as Code concepts
- HCL and the Terraform workflow
- Providers, resources, variables, dependencies, and outputs
- Local and remote state
- Lifecycle rules, data sources, and meta-arguments
- AWS resources as practical examples
- Provisioners, imports, debugging, modules, functions, and workspaces

## Prerequisites

No Terraform experience is required. Basic command-line, cloud, and DevOps knowledge is helpful.

## What Terraform does

Terraform reads declarative configuration, compares the desired configuration with its recorded state and the real infrastructure, creates an execution plan, and asks provider plugins to perform the required API operations.

Terraform itself is not a cloud provider. It communicates with platforms such as AWS, Azure, Kubernetes, GitHub, and many others through providers.

## Learning goal

By the end, you should be able to:

- Read and write basic `.tf` files.
- Understand what a plan will change.
- Safely create, update, and destroy resources.
- Keep state protected and shared correctly.
- Reuse infrastructure through modules.
- Know when Terraform is—and is not—the right tool.

## Good habits from day one

- Format and validate configuration before planning.
- Read every plan before approving it.
- Never commit credentials or local state to Git.
- Pin Terraform and provider versions deliberately.
- Prefer repeatable configuration over manual console changes.
- Use provisioners only as a last resort.

## Quick check

1. Does Terraform directly provide cloud infrastructure? **No; providers call platform APIs.**
2. Is Terraform configuration mainly declarative or imperative? **Declarative.**
3. What should you inspect before applying? **The execution plan.**

## References

- [KodeKloud course](https://learn.kodekloud.com/user/courses/terraform-basics-training-course)
- [Terraform overview](https://developer.hashicorp.com/terraform/intro)

