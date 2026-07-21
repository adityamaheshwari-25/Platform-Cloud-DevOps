# Module 00 - Introduction

## Course goal

An Azure Administrator implements, manages, secures, and monitors Azure resources. The role spans identity, governance, storage, compute, networking, backup, and monitoring. Expect to work with application, security, networking, database, and DevOps teams rather than operating in isolation.

## Recommended baseline

- Basic cloud and Azure fundamentals, roughly AZ-900 level.
- Operating systems, servers, virtualization, TCP/IP, DNS, VPNs, and storage concepts.
- Familiarity with the Azure portal plus basic PowerShell and command-line usage.

## Core administration interfaces

| Interface | Best use |
|---|---|
| Azure portal | Discovery, one-off configuration, visual troubleshooting |
| Azure Cloud Shell | Browser-based Bash or PowerShell with Azure tools available |
| Azure CLI | Cross-platform scripting and concise automation |
| Azure PowerShell | Object-oriented administration and PowerShell automation |
| ARM templates / Bicep | Repeatable, declarative infrastructure deployment |
| REST APIs / SDKs | Programmatic integration and custom tooling |

All resource operations ultimately pass through **Azure Resource Manager (ARM)**.

## Mental model for Azure scope

```text
Microsoft Entra tenant
  -> Management groups
    -> Subscriptions
      -> Resource groups
        -> Resources
```

- The Entra tenant is the identity boundary.
- A subscription is a billing, quota, and access-management boundary.
- A resource group is a logical lifecycle container for resources.
- Policies and RBAC assignments can inherit from higher scopes.

## Five exam domains

1. Identities and governance.
2. Storage.
3. Compute.
4. Virtual networking.
5. Monitoring, backup, and recovery.

## Lab habits that matter

- Use consistent names, regions, tags, and resource groups.
- Check the selected tenant and subscription before running commands.
- Prefer least privilege and private connectivity.
- Record the resource ID, private/public IPs, and effective permissions when troubleshooting.
- Delete lab resource groups when finished to control cost.
- Repeat important deployments through CLI, PowerShell, or Bicep after doing them once in the portal.

## Quick reference commands

```bash
az account show --output table
az account list --output table
az group list --output table
az resource list --output table
```

```powershell
Get-AzContext
Get-AzSubscription
Get-AzResourceGroup
Get-AzResource
```

## Exam cues

- Read the requested **scope**, **availability target**, **security requirement**, and **management overhead** before choosing a service.
- Prefer managed services when the requirement is to minimize administration.
- Distinguish the control plane (create/update/delete resources) from the data plane (access data inside a resource).
- Product names and portal labels change; focus on service behavior and decision rules.
