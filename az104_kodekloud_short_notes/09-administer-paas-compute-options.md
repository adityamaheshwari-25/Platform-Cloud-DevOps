# Module 09 - Administer PaaS Compute Options

## Choosing a compute service

| Need | Service |
|---|---|
| Full OS control | Azure Virtual Machines |
| Managed web app/API runtime | Azure App Service |
| Run a container quickly without orchestration | Azure Container Instances (ACI) |
| Serverless container platform with revisions and autoscale | Azure Container Apps |
| Full Kubernetes orchestration | AKS (outside the course's main AZ-104 focus) |

## App Service Plans

An App Service plan defines the compute resources for one or more web apps.

Key choices:

- Region and operating system.
- Pricing tier and instance size.
- Number of instances.
- Supported features such as autoscale, deployment slots, backups, and private networking.

**Scale up** changes the tier/instance size. **Scale out** changes the instance count. Multiple apps in one plan share the plan's compute capacity, so a noisy app can affect others.

## Deploying App Service

Configure:

- Runtime stack or container image.
- App settings and connection strings.
- Managed identity for access to Azure resources.
- Logging, diagnostics, Application Insights, and health check.
- Inbound and outbound networking.
- Scale rules and availability settings.

Store secrets in Key Vault and reference them through managed identity rather than placing secrets directly in app settings or code.

## Securing App Service

- Enforce HTTPS and current TLS.
- Use built-in authentication/authorization where suitable.
- Restrict inbound access using access restrictions, private endpoints, or Application Gateway/Front Door.
- Use VNet integration for outbound access from the app to VNet resources.
- Use managed identities and least-privilege RBAC.
- Configure certificates and renewal ownership.

VNet integration is primarily outbound from the app; a private endpoint provides private inbound access.

## Custom domains and certificates

1. Add the required DNS verification record.
2. Map the custom host name to the app.
3. Add or create a certificate.
4. Bind TLS to the host name.
5. Validate renewal, HTTPS redirect, and DNS behavior.

## App Service backup

Backups can include app content, configuration, and supported database content depending on the setup and plan. Store backups in a storage account, set a schedule/retention, and test restore. Backup support varies by plan and workload.

## CI/CD and deployment slots

Deployment slots are live environments with their own host names.

- Deploy and validate in staging.
- Mark environment-specific settings as slot settings.
- Swap staging into production with reduced downtime.
- Warm up the target slot before the swap.
- Roll back by swapping again when appropriate.

A slot swap changes content and selected configuration; understand which settings move and which remain sticky.

## Azure Container Instances (ACI)

ACI runs containers without managing VMs or a cluster.

- Fast startup and per-resource billing.
- Linux and Windows support depending on configuration.
- Public or private networking options.
- Environment variables, secure values, registry credentials, and volume mounts.
- Good for jobs, build agents, burst tasks, demos, and simple services.

ACI is not a full orchestrator. For complex service discovery, rolling upgrades, or large microservice platforms, use Container Apps or AKS.

## Container groups

Containers in a group share:

- Lifecycle and deployment.
- Local network namespace/IP and ports.
- Supported mounted volumes.
- Resource allocation at group/container level.

Use sidecars for helper functions such as logging or proxies when the shared lifecycle is appropriate.

## Azure Container Apps

Container Apps provides a managed environment with:

- Revisions and traffic splitting.
- HTTP/TCP ingress options.
- KEDA-based event and metric scaling, including scale to zero for suitable apps.
- Secrets, managed identity, and Dapr integration where needed.
- Internal or external environments and VNet integration options.

## Azure Container Registry (2026 exam reminder)

The current exam explicitly includes creating and managing ACR. Know repositories, images/tags, authentication, managed identity pulls, network restrictions, retention, and the relationship between ACR and ACI/Container Apps.

## Exam cues

- App Service plan = compute; App Service = hosted application.
- Scale up changes size/tier; scale out changes instance count.
- Deployment slots reduce release risk and allow swaps.
- ACI is simple container execution; Container Apps adds revisions, ingress, and autoscale.
- Private endpoint is inbound private access; VNet integration enables outbound access from App Service.
