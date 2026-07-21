# AZ-104: Microsoft Azure Administrator - Short Notes (All Modules)

Last reviewed: 2026-07-21

This combined file follows KodeKloud's course module order and is aligned to the Microsoft AZ-104 skills measured from April 17, 2026. It is a paraphrased revision aid, not a transcript.

## Table of contents

1. Introduction
2. Administer Identity
3. Administer Governance and Compliance
4. Administer Azure Resources
5. Administer Virtual Networking
6. Administer Intersite Connectivity
7. Administer Network Traffic
8. Administer Azure Storage
9. Administer Azure Virtual Machines
10. Administer PaaS Compute Options
11. Administer Data Protection
12. Administer Monitoring
13. Mock Exams and Final Review

---


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


---

# Module 01 - Administer Identity

## Microsoft Entra ID

Microsoft Entra ID is Microsoft's cloud identity and access management service. It provides authentication, authorization, single sign-on, user/group management, device identity, application identity, and external collaboration.

Key terms:

- **Tenant / directory:** An isolated Entra ID instance for an organization.
- **Identity:** A user, group, device, application, service principal, or managed identity.
- **Authentication:** Proves who or what the identity is.
- **Authorization:** Determines what the identity can access.
- **Member:** Normally belongs to the tenant.
- **Guest:** External identity invited for B2B collaboration.

## Entra ID vs Active Directory Domain Services

| Microsoft Entra ID | Active Directory Domain Services |
|---|---|
| Cloud identity and application access | Traditional Windows domain directory |
| Uses modern protocols such as OAuth, OIDC, and SAML | Uses Kerberos, NTLM, LDAP, and domain join |
| Flat tenant structure | Domains, trees, forests, and organizational units |
| Supports SaaS SSO and Conditional Access | Supports Group Policy and legacy domain workloads |
| Devices can be registered or Entra joined | Devices are domain joined |

They can coexist through hybrid identity. Microsoft Entra Domain Services is a managed domain service for workloads that still require domain protocols.

## Editions and licensing

Entra capabilities depend on licensing. Free features cover core directory functions; premium tiers add advanced identity, access, governance, and risk capabilities. Do not memorize every license feature from an old table - check the current licensing page when implementing a solution.

## Device identities

- **Entra registered:** Common for personal/BYOD devices; the user signs in with a local account but registers a work identity.
- **Entra joined:** Organization-owned, cloud-first device joined directly to Entra ID.
- **Hybrid Entra joined:** Joined to on-premises AD DS and registered with Entra ID.

Device identity is frequently combined with Intune compliance and Conditional Access.

## User accounts

Common sources:

- Cloud-only users created directly in Entra ID.
- Synchronized users originating in on-premises AD DS.
- Guest/external users invited through B2B.

Important properties include user principal name, display name, usage location, account state, group membership, assigned roles, and licenses.

Bulk operations can use CSV files, Microsoft Graph, Azure CLI, or PowerShell. Always validate the CSV headers and use a small test batch before a large import or delete.

## Groups

| Group choice | Use |
|---|---|
| Security group | Assign access to Azure resources and applications |
| Microsoft 365 group | Collaboration resources such as mailbox, SharePoint, and Teams |
| Assigned membership | Administrator explicitly manages members |
| Dynamic membership | Rules automatically add/remove users or devices; licensing required |

Assign permissions to groups rather than individual users whenever practical.

## Self-service password reset (SSPR)

SSPR lets users reset or unlock accounts after proving identity with approved methods.

Implementation checklist:

1. Select the target scope: none, selected groups, or all users.
2. Configure acceptable authentication methods and registration.
3. Define notification and security settings.
4. For hybrid users, configure password writeback when required.
5. Test with a non-administrator account.

SSPR is not the same as multifactor authentication, although both can use similar verification methods.

## Multitenant and external access

- Each tenant is an identity and policy boundary.
- B2B collaboration creates or represents an external identity in the resource tenant.
- Access is still controlled by app permissions, group membership, Conditional Access, and Azure RBAC.
- Cross-tenant access settings govern inbound and outbound trust between organizations.
- Use least privilege and review guest access regularly.

## Exam cues

- **Entra role** manages directory objects; **Azure RBAC role** manages Azure resources.
- A guest can authenticate with an identity from another organization but receives authorization in your tenant.
- Use groups for scalable assignment.
- For user sign-in problems, check account state, license, authentication method, Conditional Access, tenant, and sign-in logs.
- Know member vs guest, cloud vs synchronized identity, and registered vs joined device.


---

# Module 02 - Administer Governance and Compliance

## Azure hierarchy and inheritance

```text
Root management group
  -> Management group(s)
    -> Subscription(s)
      -> Resource group(s)
        -> Resource(s)
```

RBAC assignments and Azure Policy assignments normally inherit from parent scopes. Design higher scopes for controls that must apply broadly and lower scopes for exceptions or workload-specific configuration.

## Subscriptions and resource groups

A subscription is a boundary for billing, quotas, resource providers, and access. A resource group is a logical container used to manage a set of resources as one lifecycle unit.

Remember:

- A resource belongs to one resource group at a time.
- A resource group has a location for its metadata; its resources can be in other supported regions.
- Deleting a resource group deletes the resources in it.
- Moves between resource groups/subscriptions are service-dependent and require validation.
- Quotas and service limits are often regional or subscription-scoped; request increases before deployment.
- Resource providers may need to be registered in the subscription.

## Tags

Tags are name/value metadata for organization, ownership, environment, application, and cost reporting.

- Tags do not automatically inherit by default.
- Azure Policy can add or inherit required tags.
- Tags are not a security boundary.
- Establish a controlled tag dictionary, for example `Environment`, `Owner`, `CostCenter`, and `DataClassification`.

## Resource locks

| Lock | Effect |
|---|---|
| CanNotDelete | Resource can be modified but not deleted |
| ReadOnly | Resource cannot be modified or deleted through the control plane |

Locks inherit to child resources. They protect against accidental control-plane changes, not data-plane operations. Remove or adjust a lock before legitimate maintenance that requires changes.

## Cost management

Use:

- Cost analysis for actual and forecasted spending.
- Budgets and alerts for thresholds.
- Tags and management-group/subscription structure for allocation.
- Azure Advisor recommendations for optimization.
- Reservations, savings plans, right-sizing, auto-shutdown, and lifecycle policies where suitable.

A budget alerts you; it does not normally stop resource consumption by itself.

## Azure Policy

Azure Policy evaluates resources against organizational rules.

Core objects:

- **Definition:** The rule and effect.
- **Assignment:** Applies a definition at a scope.
- **Parameter:** Makes a definition reusable.
- **Exemption:** Documents an approved exception.
- **Compliance state:** Compliant, non-compliant, exempt, conflicting, or not started.
- **Remediation task:** Corrects supported existing resources, commonly with `modify` or `deployIfNotExists`.

Common effects include `audit`, `deny`, `append`, `modify`, `deployIfNotExists`, and `disabled`. A new deny policy prevents future non-compliant deployments but does not automatically delete existing resources.

## Initiatives

An initiative is a collection of policy definitions assigned and reported as one unit. Use initiatives for baselines such as regional restrictions, mandatory diagnostic settings, approved SKUs, and required tags.

## Azure RBAC

An RBAC assignment combines:

```text
Security principal + Role definition + Scope
```

- **Security principal:** User, group, service principal, or managed identity.
- **Role definition:** Allowed control-plane and/or data-plane actions.
- **Scope:** Management group, subscription, resource group, or resource.

Use built-in roles first. Assign at the narrowest practical scope. `Owner` can manage access, `Contributor` cannot grant roles, and `Reader` is view-only for the control plane. Data access often requires a separate data-plane role.

## Azure RBAC vs Entra roles

| Azure RBAC | Microsoft Entra roles |
|---|---|
| Controls Azure resources | Controls directory and identity objects |
| Scopes include subscription/RG/resource | Scope is tenant or administrative unit, depending on role |
| Example: Virtual Machine Contributor | Example: User Administrator |

## Exam cues

- Use **Policy** to enforce what can be deployed; use **RBAC** to control who can act.
- A lock is not a substitute for RBAC or Policy.
- Higher-scope assignments inherit downward unless excluded or otherwise overridden by supported behavior.
- For unexpected access, inspect all inherited role assignments and deny assignments.
- For unexpected policy results, check assignment scope, exclusions, parameters, effect, and compliance evaluation time.


---

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


---

# Module 04 - Administer Virtual Networking

## Virtual networks and subnets

An Azure Virtual Network (VNet) is a private logical network in Azure. A VNet contains one or more non-overlapping address prefixes and is divided into subnets.

Design rules:

- Use private RFC 1918 ranges and plan for future growth.
- Avoid overlap with on-premises networks and VNets that may later connect.
- Separate tiers and functions into subnets, such as web, application, database, management, and gateway.
- Azure reserves addresses in every subnet; do not size a subnet to the exact host count.
- Some services require a dedicated or delegated subnet.

Resources in the same VNet can normally route between subnets through Azure system routes, subject to NSGs and other controls.

## Private and public IP addresses

- A NIC receives a private IP from its subnet.
- Private IP allocation can be dynamic or static.
- A public IP can be associated with supported resources such as a NIC, load balancer, NAT Gateway, or Application Gateway.
- Prefer Standard public IPs and explicit security controls.
- Avoid public IPs on workload VMs when Bastion, private access, or a load balancer can meet the requirement.

Public IP addresses provide reachability; they do not open ports by themselves. NSGs and the operating-system firewall still apply.

## Network Security Groups (NSGs)

An NSG is a stateful layer-3/layer-4 packet filter applied to a subnet and/or NIC.

A rule includes:

- Direction: inbound or outbound.
- Source and source port.
- Destination and destination port.
- Protocol.
- Priority: lower number wins.
- Action: allow or deny.

Azure includes default rules for VNet, load balancer, and internet traffic. Custom rules are processed before lower-priority defaults. Because NSGs are stateful, return traffic for an allowed connection is automatically permitted.

Troubleshoot with **effective security rules**, **IP flow verify**, and the guest OS firewall.

## Application Security Groups (ASGs)

ASGs let you group NICs by application role and reference the group in NSG rules instead of maintaining IP address lists.

Example:

```text
Allow ASG-Web -> ASG-App on TCP 443
Allow ASG-App -> ASG-DB on TCP 1433
Deny other lateral traffic
```

ASGs improve readability and support dynamic workloads, but scope and regional constraints must be respected.

## Azure DNS

### Public DNS zones

Azure DNS hosts public DNS records. You create the zone, add records, and delegate the domain from the registrar to the Azure name servers.

Common record types: A, AAAA, CNAME, MX, TXT, NS, SRV, CAA, and alias records.

### Private DNS zones

Private DNS zones resolve names inside linked VNets without exposing records publicly.

- Link one or more VNets to the zone.
- Enable auto-registration on an appropriate link when VM records should be maintained automatically.
- A VNet can use custom DNS servers; ensure forwarders handle Azure/private zones correctly.
- Private endpoints usually require correct private DNS zone integration.

## Quick troubleshooting sequence

1. Confirm source and destination IPs and subnets.
2. Check route tables and effective routes.
3. Check NSGs on both subnet and NIC.
4. Check the OS firewall and whether the process is listening.
5. Check DNS resolution.
6. Use Network Watcher connection troubleshooting or packet capture when needed.

## Exam cues

- VNet address spaces may overlap while isolated, but cannot be directly peered until overlap is removed.
- NSGs filter traffic; route tables choose the path.
- A public IP does not replace an NSG rule.
- Use ASGs to express application roles in NSG rules.
- Public DNS is internet-facing; private DNS is linked to VNets.


---

# Module 05 - Administer Intersite Connectivity

## Connectivity choices

| Requirement | Typical service |
|---|---|
| Low-latency VNet-to-VNet traffic over Microsoft backbone | VNet peering |
| Encrypted VNet-to-VNet tunnel through gateways | VPN Gateway VNet-to-VNet |
| On-premises site to Azure over the internet | Site-to-Site (S2S) VPN |
| Individual client to Azure | Point-to-Site (P2S) VPN |
| Private, predictable enterprise circuit | ExpressRoute |

Choose based on topology, encryption, bandwidth, latency, availability, cost, and operational effort.

## Virtual Network Peering

Peering connects two VNets so resources communicate through private IP addresses over the Microsoft backbone.

Important characteristics:

- Address spaces must not overlap.
- Peering can be regional or global.
- Peering is **not transitive**. If A peers with B and B peers with C, A does not automatically reach C.
- You can allow forwarded traffic for hub-and-spoke designs.
- Network access and resource access settings exist on each side; a complete peering normally configures both directions.
- DNS configuration is separate from connectivity.

## VPN Gateway

A VPN Gateway is deployed into a subnet named exactly `GatewaySubnet`.

- **Route-based VPN:** Preferred for most modern and multi-site scenarios.
- **Policy-based VPN:** Uses defined traffic selectors and is more limited.
- Gateway SKUs determine throughput, tunnel count, zone support, and features.
- Active-active and zone-redundant designs can improve availability where supported.

### Site-to-Site

Connects an on-premises VPN device to an Azure VPN Gateway using IPsec/IKE. Requires compatible public IPs, non-overlapping networks, and matching cryptographic/routing configuration.

### Point-to-Site

Connects individual clients to the VNet. Authentication can use certificates, Microsoft Entra ID, or RADIUS depending on protocol and configuration.

## Gateway transit

Gateway transit lets a spoke VNet use a VPN/ExpressRoute gateway in a peered hub.

Typical settings:

- Hub peering: allow gateway transit.
- Spoke peering: use remote gateways.

A VNet cannot simultaneously use its own gateway and a remote gateway in the same way. Peering remains non-transitive, so hub routing and forwarding must be designed explicitly.

## User-defined routes (UDRs)

Azure supplies system routes. A route table lets you override or extend routing for a subnet.

Common next hops:

- Virtual network gateway.
- Virtual network.
- Internet.
- Virtual appliance.
- None (drop traffic).

Azure selects the most specific matching prefix. UDRs are commonly used to force traffic through a firewall or network virtual appliance (NVA). Ensure the NVA can forward traffic and that return routing is symmetric.

## Service endpoints vs private endpoints

| Service endpoint | Private endpoint |
|---|---|
| Service still uses its public endpoint | Service receives a private IP in your VNet |
| Extends VNet/subnet identity to supported PaaS service | Uses Private Link for private connectivity |
| DNS usually resolves to public address | DNS must resolve service name to private IP |
| Simpler and often lower cost | Stronger isolation and on-premises/private access |
| Access can be limited to selected VNets/subnets | Access can disable or restrict public network access |

A private endpoint is a NIC in your VNet; secure it by controlling the target service and DNS, not by treating it like a normal VM NIC.

## Exam cues

- Peering is fast and private but non-transitive.
- VPN Gateway requires `GatewaySubnet`; do not deploy unrelated workloads there.
- Service endpoints secure access to the public service endpoint; private endpoints bring the service into the VNet through a private IP.
- UDRs control path selection; NSGs control whether traffic is allowed.
- For a failed private endpoint connection, check approval state, private DNS, routes, and public-network settings.


---

# Module 06 - Administer Network Traffic

## Service selection

| Service | Layer / scope | Best for |
|---|---|---|
| Azure Load Balancer | Layer 4, regional | TCP/UDP distribution to backend instances |
| Application Gateway | Layer 7, regional | HTTP/HTTPS routing, TLS termination, WAF |
| Azure Front Door | Layer 7, global | Global web acceleration, routing, and edge security |
| Traffic Manager | DNS, global | DNS-based endpoint selection across regions/providers |

## Azure Load Balancer

Load Balancer distributes TCP/UDP flows to healthy backend instances.

Core components:

- **Frontend IP configuration:** Public or internal IP receiving traffic.
- **Backend pool:** NICs, VM scale sets, or supported IP-based backends.
- **Health probe:** Determines whether a backend can receive new flows.
- **Load-balancing rule:** Maps frontend IP/port/protocol to backend port and pool.
- **Inbound NAT rule:** Maps a frontend port to a specific backend instance.
- **Outbound rule:** Controls scalable outbound SNAT for supported configurations.

A health probe failure stops new connections to the unhealthy instance; it does not repair the application.

## Session persistence

Load distribution can use:

- **None:** Five-tuple hash; best general distribution.
- **Client IP:** Two-tuple affinity.
- **Client IP and protocol:** Three-tuple affinity.

Use affinity only when the application requires it. Prefer application-level session storage for scalable web applications.

## Application Gateway

Application Gateway is a regional HTTP/HTTPS reverse proxy and load balancer.

Key capabilities:

- Host-based and path-based routing.
- TLS termination and end-to-end TLS.
- Cookie-based session affinity.
- Redirects and URL rewrite.
- Autoscaling and zone redundancy on supported SKUs.
- Web Application Firewall (WAF) on WAF SKUs.

Core components:

```text
Frontend IP -> Listener -> Routing rule -> Backend pool
                              |-> Backend setting
                              |-> Health probe
```

- A listener matches protocol, port, host name, and certificate.
- A rule links a listener to a backend pool and settings or to a redirect.
- Custom probes should test a meaningful application path.

## Other load-balancing choices

- Use **Front Door** for global HTTP/S routing and edge acceleration.
- Use **Traffic Manager** when DNS-based global distribution is sufficient, including non-HTTP endpoints.
- Use **NAT Gateway** for predictable, scalable outbound internet connectivity from subnets; it is not an inbound load balancer.

## Network Watcher

Network Watcher provides regional network monitoring and troubleshooting tools, including:

- IP flow verify.
- Next hop.
- Effective security rules and routes.
- Connection troubleshoot / Connection Monitor.
- Packet capture.
- Topology and reachability diagnostics.
- Flow logging capabilities where supported.

Use these tools to locate whether failure is caused by DNS, routes, NSGs, load-balancer probes, the guest firewall, or the application itself.

## Troubleshooting a load-balanced application

1. Confirm the frontend IP and listener/rule.
2. Verify the backend pool contains the expected instances.
3. Check probe protocol, port, path, host header, and backend response.
4. Verify NSGs allow probe and application traffic.
5. Confirm the application is listening on the backend port.
6. Check routes, firewalls, and asymmetric routing.
7. Review metrics and diagnostic logs.

## Exam cues

- Load Balancer is Layer 4; Application Gateway is Layer 7.
- An internal load balancer has a private frontend IP; a public load balancer has a public frontend IP.
- A probe determines health, while a rule determines traffic mapping.
- Application Gateway supports content-aware routing and optional WAF.
- Global web routing normally points toward Front Door; DNS-only global routing points toward Traffic Manager.


---

# Module 07 - Administer Azure Storage

## Storage account basics

A storage account is the namespace and management boundary for Azure Storage services.

Main services:

- **Blob:** Unstructured object data.
- **Files:** Managed SMB/NFS file shares.
- **Queue:** Asynchronous messages.
- **Table:** NoSQL key/attribute data.
- **Disks:** Managed block storage for Azure VMs.

General-purpose v2 (`StorageV2`) is the default account type for most standard workloads. Premium account types target specific high-performance services.

## Redundancy

| Option | Copies / scope | Main use |
|---|---|---|
| LRS | Multiple copies in one datacenter | Lowest cost, local durability |
| ZRS | Synchronous copies across zones in one region | Zone resilience |
| GRS | LRS in primary plus asynchronous secondary region | Regional disaster protection |
| RA-GRS | GRS plus read access to secondary | Read from secondary during normal operation |
| GZRS | ZRS primary plus asynchronous secondary region | Zone and regional protection |
| RA-GZRS | GZRS plus read access to secondary | Highest read-access option among these |

Geo-replication is asynchronous, so the recovery point can lag. Choose redundancy based on RPO/RTO, regional support, cost, and workload requirements.

## Storage endpoints and network security

A storage account has service endpoints such as blob and file endpoints.

Security controls:

- Disable anonymous blob access unless explicitly required.
- Require secure transfer and a modern minimum TLS version.
- Use the storage firewall to allow selected networks/IPs.
- Use service endpoints for subnet-restricted access to the public endpoint.
- Use private endpoints for private IP connectivity and private DNS integration.
- Disable public network access when private-only access is required.

## Blob containers and blob types

A blob container groups blobs within a storage account.

- **Block blob:** Documents, media, backups, and general objects.
- **Append blob:** Append-heavy logging scenarios.
- **Page blob:** Random read/write pages; used by VHD-style workloads.

Container access should normally be private. Authorization should be explicit.

## Access tiers and lifecycle management

- **Hot:** Frequent access; higher storage cost, lower access cost.
- **Cool / Cold:** Infrequent access with minimum retention and higher access cost.
- **Archive:** Offline tier with low storage cost and rehydration delay.

A lifecycle policy can move blobs between tiers or delete old versions/snapshots based on age, prefix, tags, and other supported filters. Test policies carefully because deletion can be irreversible after retention features expire.

## Azure Files

Azure Files provides managed file shares over SMB and, for supported configurations, NFS.

Know how to:

- Create a share and set quota/tier.
- Mount from Windows or Linux.
- Use snapshots and soft delete.
- Configure identity-based access where required.
- Use Azure File Sync to cache an Azure file share on Windows Server.

## Encryption

- Storage Service Encryption encrypts data at rest by default.
- Keys can be Microsoft-managed or customer-managed through Key Vault where supported.
- Infrastructure encryption can add a second encryption layer for supported services.
- Azure Disk Encryption uses BitLocker or dm-crypt inside supported VMs; encryption at host protects data at the host layer. These are different controls.

## Authorization choices

| Method | Guidance |
|---|---|
| Microsoft Entra ID + Azure RBAC | Preferred for identity-based, least-privilege access |
| User delegation SAS | Delegated temporary access authorized through Entra ID |
| Service/account SAS | Scoped and time-limited access; protect the token |
| Account access keys | Broad power; rotate and avoid application embedding |

A SAS should specify only the required service/resource, permissions, start/expiry time, protocol, and optional IP range. A stored access policy can help manage service SAS behavior for supported services.

## Data-management tools

- Azure portal and Storage browser.
- Azure Storage Explorer.
- AzCopy for high-performance transfer.
- Azure CLI and PowerShell.

```bash
azcopy copy './data/*' 'https://ACCOUNT.blob.core.windows.net/CONTAINER?<SAS>' --recursive
```

## Exam cues

- Redundancy protects availability/durability; backup protects against logical deletion and retention requirements.
- Prefer Entra ID and RBAC over access keys.
- Service endpoint = public endpoint restricted to VNet; private endpoint = private IP.
- Know soft delete, versioning, snapshots, lifecycle management, object replication, and access tiers.
- For access failure, check identity role, SAS validity, firewall/private endpoint, DNS, protocol, and storage service path.


---

# Module 08 - Administer Azure Virtual Machines

## Planning VMs

Azure VMs are Infrastructure as a Service. Microsoft manages facilities, physical hardware, and the hypervisor; you manage the guest OS, applications, data, patching approach, identity, network controls, and most workload security.

Plan:

- Region and availability requirement.
- VNet, subnet, DNS, NSG, private/public access.
- Image, generation, architecture, and OS support.
- VM size, CPU/memory ratio, accelerated networking, and regional availability.
- OS/data disk performance and capacity.
- Authentication, managed identity, backup, monitoring, updates, and cost model.

## VM sizes

Size families target general purpose, compute, memory, storage, GPU, and high-performance workloads. Resizing may require a restart, and not every size is available on the current cluster or in every region.

Use metrics to right-size. Check quota before scaling or changing family.

## VM storage

- **OS disk:** Boot volume; persistent managed disk.
- **Data disk:** Persistent managed disk attached to the VM.
- **Temporary disk:** Local host storage; data can be lost during maintenance/redeploy. Never store durable data here.
- **Ephemeral OS disk:** OS disk on local VM storage for suitable stateless workloads; fast reimage but not persistent like a managed OS disk.

Managed disk options include Standard HDD, Standard SSD, Premium SSD, and Ultra Disk where supported. Performance depends on both disk and VM limits.

## Creating a VM

Core configuration:

1. Subscription, resource group, region, name, and availability choice.
2. Security type, image, architecture, and size.
3. Admin authentication: SSH key preferred for Linux; strong credentials and secure access for Windows.
4. OS/data disks and encryption settings.
5. VNet, subnet, NIC, NSG, public IP/load balancer.
6. Managed identity, diagnostics, backup, auto-shutdown, patching, and extensions.
7. Tags and review of generated ARM/Bicep when useful.

Current exam objectives also include moving a VM between supported resource groups, subscriptions, or regions and configuring encryption at host.

## Connecting securely

| OS | Common method |
|---|---|
| Linux | SSH, normally TCP 22 |
| Windows | RDP, normally TCP 3389 |
| Either | Azure Bastion through browser/native client options |

Prefer Azure Bastion, VPN, private connectivity, or a controlled jump host over direct public exposure. Just-in-time access can reduce the time management ports are open where Defender for Cloud features are used.

## High availability

### Availability sets

- Spread VMs across fault domains and update domains within a datacenter scope.
- Useful for workloads not deployed across availability zones.
- VMs must be deliberately placed in the set during deployment.

### Availability zones

- Physically separate datacenters within a region.
- Protect against a zone failure.
- Design data, networking, and load balancing for zone resilience too.

### Virtual Machine Scale Sets (VMSS)

VMSS deploys and manages a group of similar VMs.

- Supports autoscaling by metric or schedule.
- Integrates with Load Balancer or Application Gateway.
- Uses an image/model and upgrade policy.
- Instances should be stateless or use external durable state.
- Health monitoring and automatic repairs can replace unhealthy instances.

## Operational tasks

- Stop vs deallocate: deallocation releases compute billing and can change a dynamic public IP.
- Capture reusable images through Azure Compute Gallery for controlled versioning.
- Use extensions for post-deployment configuration, but keep scripts idempotent.
- Back up before risky disk or OS changes.
- Monitor guest and platform metrics separately.

## Exam cues

- Availability set protects against host/rack maintenance/failure; zones protect against datacenter-zone failure.
- A temporary disk is not durable.
- Bastion avoids exposing RDP/SSH directly through a VM public IP.
- VMSS is for repeated instances and scaling, not merely one large VM.
- Check both the VM size limit and disk limit when troubleshooting storage performance.


---

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


---

# Module 10 - Administer Data Protection

## Backup vs disaster recovery

| Azure Backup | Azure Site Recovery (ASR) |
|---|---|
| Creates recoverable restore points | Replicates workloads for failover |
| Protects against deletion, corruption, ransomware, and retention needs | Protects service continuity during site/region failure |
| Restore files, disks, VMs, or supported workloads | Fail over and later fail back/reprotect |
| RPO/RTO depends on backup schedule and restore process | RPO/RTO depends on continuous replication and recovery plan |

Many production systems need both.

## Vaults

- **Recovery Services vault:** Commonly manages Azure VM backup, MARS/MABS/DPM backup, and Azure Site Recovery.
- **Backup vault:** Used for newer Azure Backup workloads and capabilities. Supported data sources differ from a Recovery Services vault.

Choose the correct vault type, region, subscription, redundancy, security settings, and identity before enabling protection.

## Azure Backup flow

1. Create the appropriate vault.
2. Configure redundancy and security controls.
3. Create/select a backup policy with frequency and retention.
4. Enable backup for the data source.
5. Run or wait for the first backup.
6. Monitor jobs and alerts.
7. Test restore and document recovery steps.

A backup is only useful when restore has been tested.

## File and folder backup

The Microsoft Azure Recovery Services (MARS) agent can protect supported Windows files, folders, volumes, and system state directly to a Recovery Services vault.

- Install and register the agent with vault credentials.
- Set encryption/passphrase securely; Microsoft cannot recover a lost passphrase.
- Configure schedule, retention, bandwidth, and exclusions.
- Restore to the original or alternate location.

For larger server/application environments, Microsoft Azure Backup Server (MABS) or System Center DPM may be appropriate.

## Azure VM backup

Azure VM backup uses a VM extension and snapshots/transfer to create recovery points in the vault.

- Policy controls schedule and retention.
- Recovery points can be application-consistent, file-system-consistent, or crash-consistent depending on workload and conditions.
- Restore options can include a full VM, disks, or individual files.
- Ensure networking, encryption keys, identities, and dependencies are available during restore.

## Non-Azure VM backup

Options include:

- MARS for files/folders/system state on supported Windows machines.
- MABS/DPM for broader workloads.
- Azure Arc integration and service-specific solutions where supported.

Connectivity, proxy/firewall rules, credentials, and bandwidth are common failure points.

## Soft delete and security

Soft delete retains deleted backup data for a recovery period, helping defend against accidental or malicious deletion.

Also consider:

- Immutability where supported.
- Multi-user authorization for critical operations.
- Resource Guard and least-privilege backup roles.
- Alerts for disabled protection, deleted backup items, and failed jobs.
- Separate operational and security ownership for destructive actions.

## Azure Site Recovery

ASR replicates supported workloads to a recovery location.

Core sequence:

1. Define source and target resources.
2. Enable replication and select replication policy.
3. Map networks and configure target compute/storage settings.
4. Monitor replication health.
5. Run a **test failover** in an isolated network.
6. During an incident, perform planned or unplanned failover as appropriate.
7. Validate, commit, and then reprotect for failback/reverse replication.

Recovery plans can group machines, define order, and run automation steps.

## Exam cues

- Backup is retention/recovery; ASR is replication/failover.
- Know when to use a Recovery Services vault versus a Backup vault.
- Soft delete protects deleted backup data, not the original live resource by itself.
- Test failover should not disrupt production and should use an isolated network.
- Monitor backup jobs, reports, alerts, restore readiness, and ASR replication health.


---

# Module 11 - Administer Monitoring

## Azure Monitor model

Azure Monitor collects telemetry from applications, operating systems, Azure resources, subscriptions, tenants, on-premises systems, and other clouds.

```text
Data sources
  -> collection (platform, diagnostic settings, AMA/DCR, APIs)
    -> data platform (metrics, logs, traces, changes)
      -> analysis/visualization/response
```

Consumers include Metrics Explorer, Log Analytics, Workbooks, Insights, dashboards, Grafana, alerts, autoscale, Logic Apps, Functions, and other integrations.

## Metrics vs logs

| Metrics | Logs |
|---|---|
| Numeric time-series data | Structured records in tables |
| Near real-time and efficient for alerting | Rich context and flexible correlation |
| Common for CPU, latency, requests, capacity | Common for events, diagnostics, audit, and application records |
| Queried with metric tools | Queried with Kusto Query Language (KQL) |

Use metrics for fast threshold detection and logs for deeper investigation.

## Activity Log and resource logs

- **Azure Activity Log:** Subscription-level control-plane events such as create, update, delete, policy, service health, and administrative actions.
- **Resource logs:** Service-specific data-plane/operational logs. They are not all retained centrally unless diagnostic settings route them.
- **Entra logs:** Sign-in and audit events from Microsoft Entra ID.

A diagnostic setting can send supported logs/metrics to Log Analytics, Storage, Event Hubs, or partner destinations.

## Log Analytics workspace

A workspace stores and queries Azure Monitor Logs.

Design considerations:

- Region, data residency, access model, retention, cost, and network access.
- Centralized vs distributed workspace strategy.
- Table-level ingestion and retention needs.
- RBAC for workspace and resource-context access.

### KQL basics

```kusto
Heartbeat
| summarize LastHeartbeat=max(TimeGenerated) by Computer
| order by LastHeartbeat desc
```

```kusto
AzureActivity
| where TimeGenerated > ago(24h)
| where ActivityStatusValue == "Failure"
| project TimeGenerated, OperationNameValue, ResourceGroup, Caller
| order by TimeGenerated desc
```

```kusto
Perf
| where TimeGenerated > ago(1h)
| where CounterName == "% Processor Time"
| summarize AvgCPU=avg(CounterValue) by Computer, bin(TimeGenerated, 5m)
| render timechart
```

Common operators: `where`, `project`, `extend`, `summarize`, `count`, `distinct`, `join`, `sort/order`, `top`, and `render`.

## Azure Monitor Agent and DCRs

Use **Azure Monitor Agent (AMA)** for supported VM/server telemetry. AMA uses **Data Collection Rules (DCRs)** to define:

- Which machines/resources are associated.
- Which data sources to collect.
- Transformations where supported.
- Destination workspaces/endpoints.

For non-Azure servers, Azure Arc commonly provides the Azure resource representation needed for management.

The legacy Log Analytics Agent/Microsoft Monitoring Agent (MMA) was retired on August 31, 2024. Microsoft states that data upload can stop after March 2, 2026. Do not design new monitoring around MMA.

## Azure Alerts

An alert rule contains:

1. **Scope:** Resource(s) being monitored.
2. **Condition:** Signal and trigger logic.
3. **Actions:** One or more action groups.
4. **Details:** Name, severity, enablement, identity, and other settings.

Alert types include metric, log search, activity log, and service health alerts.

Action groups can send email/SMS/push/voice notifications or trigger webhooks, Functions, Logic Apps, Automation, ITSM, and other integrations.

Alert processing rules can suppress notifications or apply action groups during maintenance and other scenarios without rewriting every alert rule.

## Insights and network monitoring

Azure Monitor Insights provides curated monitoring for VMs, storage, networks, containers, and other supported services. For networking, know Network Watcher and Connection Monitor for reachability, latency, topology, and troubleshooting.

## Troubleshooting missing telemetry

1. Confirm the resource supports the desired metric/log.
2. Check diagnostic settings or DCR association.
3. Verify AMA/extension health and managed identity permissions.
4. Check destination workspace and table.
5. Expand the query time range and verify filters.
6. Check ingestion latency, network restrictions, and data caps/transformations.

## Exam cues

- Activity Log = subscription control-plane events; resource logs = service-specific logs.
- Metrics are numerical time series; logs are queried with KQL.
- AMA + DCR is the current agent model.
- Alert rule decides when; action group decides what happens.
- Diagnostic settings route data; a Log Analytics workspace stores/query logs.


---

# Module 12 - Mock Exams and Final Review

## Purpose of mock exams

Mock exams are diagnostic tools. The goal is not to memorize an answer key; it is to identify weak decision rules, missed constraints, and slow topics.

## Three-pass approach

### Pass 1 - Timed attempt

- Use exam-like timing and no notes.
- Mark uncertain questions instead of repeatedly rereading them.
- Record confidence: high, medium, or low.

### Pass 2 - Error analysis

For every wrong or low-confidence question, classify the cause:

| Cause | Corrective action |
|---|---|
| Concept gap | Re-read the relevant module and official docs |
| Service-selection gap | Build a comparison table and one scenario |
| Scope/inheritance mistake | Draw tenant -> MG -> subscription -> RG -> resource |
| Portal/command gap | Repeat the lab from memory |
| Misread requirement | Highlight words such as private, global, zone, least privilege, minimize cost |
| Outdated detail | Verify against current Microsoft documentation |

### Pass 3 - Rebuild and retest

- Recreate the weak scenario in a lab.
- Explain the solution aloud without looking at notes.
- Retake only after you can state why each alternative is wrong.

## High-value comparison list

Be able to choose quickly between:

- Entra role vs Azure RBAC role.
- RBAC vs Azure Policy vs resource lock.
- Service endpoint vs private endpoint.
- NSG vs UDR vs Azure Firewall/NVA.
- VNet peering vs VPN Gateway vs ExpressRoute.
- Load Balancer vs Application Gateway vs Front Door vs Traffic Manager.
- Availability set vs availability zone vs VMSS.
- App Service vs ACI vs Container Apps vs VM.
- Access key vs SAS vs Entra ID/RBAC.
- LRS vs ZRS vs GRS/GZRS.
- Azure Backup vs Site Recovery.
- Activity Log vs resource logs vs metrics.
- Alert rule vs action group vs alert processing rule.

## Final hands-on checklist

From a blank subscription, practice this sequence:

1. Create a resource group and apply tags.
2. Create users/groups and assign a least-privilege role.
3. Apply a policy and a delete lock.
4. Deploy a VNet with subnets, NSGs, and private DNS.
5. Deploy a VM without direct public management ports; connect through Bastion.
6. Create a storage account with private access, lifecycle, soft delete, and Entra authorization.
7. Deploy an App Service or container workload with managed identity.
8. Configure backup and perform a test restore.
9. Send logs to Log Analytics, run KQL, and create an alert/action group.
10. Export or recreate part of the environment with Bicep.

## Exam technique

- Identify the required outcome first, then eliminate services that fail a hard constraint.
- Watch for scope and inheritance.
- Prefer least privilege and private access.
- Do not add a new service when an existing configuration change meets the requirement.
- Distinguish high availability, backup, and disaster recovery.
- Recheck whether a question asks for the lowest cost, least administration, fastest recovery, or strongest isolation.

## Readiness signal

You are close to ready when you can consistently explain not only the correct answer, but also why each plausible alternative fails one requirement.


---
