# Module 10 — Resource Deployment Tools

## Tools for interacting with Azure

| Tool | Style | Best use |
|---|---|---|
| Azure portal | Browser GUI | Learning, exploration, and occasional administration |
| Azure PowerShell | PowerShell cmdlets | PowerShell-based automation and administration |
| Azure CLI | Cross-platform command line | Shell scripts and command-line automation |
| Azure Cloud Shell | Browser-hosted shell | Using PowerShell or Azure CLI without local installation |

Examples of Azure CLI commands:

```bash
az login
az group list --output table
```

Repeated manual portal deployment can create inconsistency. Scripts and declarative templates improve repeatability.

## Azure Resource Manager (ARM)

ARM is Azure’s deployment and management control plane.

- Receives requests from the portal, CLI, PowerShell, SDKs, and REST APIs.
- Authenticates and authorizes the request.
- Sends the operation to the correct **resource provider**.
- Applies management features such as RBAC, Policy, tags, and locks.

Resource-provider examples include:

- `Microsoft.Compute`
- `Microsoft.Storage`
- `Microsoft.Network`
- `Microsoft.Web`

## Infrastructure as Code (IaC)

IaC defines infrastructure in version-controlled files.

Benefits:

- Repeatable deployments
- Consistent environments
- Peer review and version history
- Easier automation
- Reduced manual configuration drift

| Technology | Format | Scope |
|---|---|---|
| ARM templates | Declarative JSON | Azure-native |
| Bicep | Declarative Azure language | Azure-native; compiles to ARM templates |
| Terraform | Declarative HCL | Multicloud and many providers |

**Declarative** means describing the desired final state. The deployment engine determines how to reach it.

## Azure Arc

Azure Arc extends Azure management and governance to resources outside Azure.

It can help manage supported:

- On-premises servers
- Resources in other clouds
- Kubernetes clusters
- Data services and edge environments

Arc-enabled resources can appear in Azure Resource Manager and use capabilities such as tags, Azure Policy, inventory, monitoring, and security tools.

Azure Arc does not move a server into Azure; it connects the resource to Azure’s management plane.

## Exam traps

- Cloud Shell is an environment that provides PowerShell and Azure CLI; it is not a separate management API.
- Bicep is Azure-focused and compiles to ARM; Terraform is commonly used across providers.
- All common Azure management interfaces ultimately work through ARM.
- Azure Arc manages external resources from Azure; it is not itself a migration service.

## Quick check

1. Which browser tool requires no local CLI installation? **Azure Cloud Shell.**
2. What is Azure’s deployment and management control plane? **Azure Resource Manager.**
3. Which Azure-native language is more concise than ARM JSON? **Bicep.**
4. Which service extends Azure governance to on-premises and multicloud resources? **Azure Arc.**

## References

- [KodeKloud — Resource Deployment Tools](https://notes.kodekloud.com/docs/Microsoft-Azure-Fundamentals-AZ-900/Resource-Deployment-Tools/Introduction)
- [KodeKloud — Azure Resource Manager](https://notes.kodekloud.com/docs/Microsoft-Azure-Fundamentals-AZ-900/Resource-Deployment-Tools/Azure-Resource-Manager)
- [KodeKloud — Infrastructure as Code](https://notes.kodekloud.com/docs/Microsoft-Azure-Fundamentals-AZ-900/Resource-Deployment-Tools/Infrastructure-as-Code)

