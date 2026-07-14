# Module 5 — Compute and Networking

## Compute choices

| Service | Model | Best fit |
|---|---|---|
| Virtual Machines | IaaS | Full OS control or legacy/custom workloads |
| Virtual Machine Scale Sets | IaaS | Many similar VMs with automatic scaling |
| Azure Virtual Desktop | Managed desktop virtualization | Secure remote Windows desktops and applications |
| Azure App Service | PaaS | Managed web applications and APIs |
| Azure Container Instances | Container service | Fast, simple container execution without orchestration |
| Azure Container Apps | Serverless containers | Event-driven apps and microservices |
| Azure Kubernetes Service | Managed Kubernetes | Complex container orchestration |
| Azure Functions | Serverless | Short, event-driven code |

## Virtual Machines

- Provide control over the guest operating system and installed software.
- The customer is responsible for OS patching, security configuration, and applications.
- Suitable for lift-and-shift migrations and workloads that need OS-level control.

### VM availability

- **Availability set:** distributes VMs across fault domains and update domains.
- **Fault domain:** separate hardware/power failure boundary.
- **Update domain:** group that may be restarted together during planned maintenance.
- **Availability zone:** physically separate data-center location in a region.

## Virtual Machine Scale Sets

- Deploy and manage a group of similar VMs.
- Scale the number of instances manually or automatically.
- Commonly distribute traffic using a load balancer.
- Good for variable, stateless workloads that still need VMs.

## Azure Virtual Desktop (AVD)

- Delivers Windows desktops and individual applications remotely.
- Supports multi-session Windows experiences.
- Integrates with Microsoft Entra ID and Microsoft 365.
- Keeps apps and data in Azure while users connect from different devices.

## Azure App Service

- Managed platform for web apps, REST APIs, and back ends.
- Supports common languages and deployment methods.
- Handles OS maintenance and platform patching.
- Supports scaling, custom domains, TLS, deployment slots, and CI/CD integration.

Choose App Service when you need a managed web platform and do not require guest-OS control.

## Containers

Containers package an application and its dependencies. They share the host OS kernel, so they normally start faster and use fewer resources than VMs.

| Service | Use when |
|---|---|
| Azure Container Instances (ACI) | You need a container quickly without managing servers or an orchestrator |
| Azure Container Apps | You need serverless microservices, revisions, or event-based scaling |
| Azure Kubernetes Service (AKS) | You need Kubernetes orchestration and advanced control |

## Azure Functions

- Runs code in response to a trigger.
- Common triggers include HTTP requests, timers, queues, and data changes.
- Automatically scales and commonly charges based on execution in a consumption plan.
- Best for focused event-driven work, not a traditional always-running server.

## Virtual networking

An **Azure Virtual Network (VNet)** is a private network boundary for Azure resources.

- **Subnet:** divides a VNet into smaller address ranges.
- **VNet peering:** privately connects two VNets through Microsoft’s backbone.
- **Network security group (NSG):** allows or denies inbound and outbound network traffic using rules.
- **Public IP:** enables internet-reachable addressing when required.
- **Private IP:** supports internal communication within connected private networks.

## Hybrid connectivity

| Service | Connection | Main characteristic |
|---|---|---|
| VPN Gateway | Encrypted tunnel over the public internet | Faster and less expensive to establish |
| ExpressRoute | Private dedicated connection through a connectivity provider | More predictable performance and does not traverse the public internet |

- **Site-to-site VPN:** connects an on-premises network to an Azure VNet.
- **Point-to-site VPN:** connects an individual device to an Azure VNet.

## Azure DNS

- Hosts DNS zones and records on Azure’s global infrastructure.
- Translates domain names into IP addresses.
- Can manage public DNS zones and private DNS zones for VNets.
- Azure DNS hosts DNS records; it does not sell domain names.

## Exam traps

- Containers are not miniature VMs; they share a host kernel.
- Functions are event-driven code, while App Service hosts complete web applications and APIs.
- VPN Gateway uses the public internet; ExpressRoute provides private connectivity.
- VNet peering connects VNets; it is not a VPN to a user device.
- Azure DNS hosts domains but is not a domain registrar.

## Quick check

1. Need complete control of Windows or Linux? **Azure Virtual Machines.**
2. Need a managed web app without OS patching? **Azure App Service.**
3. Need short code that runs when a queue receives a message? **Azure Functions.**
4. Need private, dedicated on-premises connectivity? **ExpressRoute.**

## References

- [KodeKloud — Compute and Networking](https://notes.kodekloud.com/docs/Microsoft-Azure-Fundamentals-AZ-900/Compute-and-Networking/Introduction)
- [KodeKloud — Compare Compute Services](https://notes.kodekloud.com/docs/Microsoft-Azure-Fundamentals-AZ-900/Compute-and-Networking/Compare-Compute-Services)
- [KodeKloud — Virtual Networking](https://notes.kodekloud.com/docs/Microsoft-Azure-Fundamentals-AZ-900/Compute-and-Networking/Virtual-Networking)

