# Module 03 - Administer Azure Resources

## Choosing an administrator tool

| Tool | Strength |
|---|---|
| Portal | Interactive setup and visual inspection |
| Cloud Shell | Quick browser-based CLI/PowerShell work |
| Azure CLI | Cross-platform automation and compact JSON-friendly output |
| Azure PowerShell | PowerShell objects, pipelines, and administrative scripts |
| ARM template | Native declarative JSON deployment |
| Bicep | Cleaner declarative language compiled to ARM JSON |
| REST/SDK | Application integration and custom automation |

Use a declarative template for repeatability. Use CLI/PowerShell for orchestration, queries, and operational tasks.

## Azure Resource Manager (ARM)

ARM is Azure's control-plane management layer. The portal, CLI, PowerShell, SDKs, and REST clients send requests to ARM, which authenticates and authorizes the caller before forwarding the request to the resource provider.

ARM enables:

- Resource groups and consistent resource IDs.
- RBAC, tags, locks, and Policy.
- Declarative deployments.
- Dependency tracking and parallel deployment where possible.
- Deployment history and what-if analysis.

## ARM templates

An ARM template is JSON that declares the desired infrastructure.

Main sections:

```json
{
  "$schema": "...",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {},
  "functions": [],
  "resources": [],
  "outputs": {}
}
```

- **Parameters:** Values supplied at deployment time.
- **Variables:** Reusable calculated values.
- **Resources:** Resource types, API versions, names, locations, properties, dependencies, and loops.
- **Outputs:** Values returned after deployment.

Prefer secure parameter handling through Key Vault or deployment tooling; never store secrets in source control.

## Deployment behavior

- Deployments are intended to be idempotent: repeating the same declaration should converge on the same state.
- **Incremental mode** adds or updates declared resources and leaves undeclared resources in place.
- **Complete mode** can remove undeclared resources at the deployment scope; use with extreme caution.
- Use `what-if` before production changes.
- Use symbolic dependencies in Bicep or `dependsOn` only when a real dependency exists.
- Choose a recent, supported API version for each resource type.

## Bicep

Bicep provides concise syntax, type checking, modules, loops, conditions, parameters, and outputs while deploying through ARM.

```bicep
param location string = resourceGroup().location
param storageName string

resource storage 'Microsoft.Storage/storageAccounts@2023-05-01' = {
  name: storageName
  location: location
  sku: { name: 'Standard_LRS' }
  kind: 'StorageV2'
  properties: {
    allowBlobPublicAccess: false
    minimumTlsVersion: 'TLS1_2'
  }
}

output storageId string = storage.id
```

Use modules to separate networking, compute, storage, and monitoring into reusable units.

## Useful commands

```bash
az deployment group what-if   --resource-group rg-demo   --template-file main.bicep   --parameters storageName=mystorage123

az deployment group create   --resource-group rg-demo   --template-file main.bicep   --parameters storageName=mystorage123
```

```powershell
New-AzResourceGroupDeployment `
  -ResourceGroupName rg-demo `
  -TemplateFile .\main.bicep `
  -storageName mystorage123
```

## Exam cues

- Know how to interpret and modify an existing ARM template or Bicep file.
- Parameters vary by environment; variables reduce repetition; outputs expose deployment results.
- A template deploys to a scope: resource group, subscription, management group, or tenant.
- ARM is the control plane, not the runtime/data path of the service.
- Validate tenant, subscription, scope, API version, dependencies, and permissions when a deployment fails.
