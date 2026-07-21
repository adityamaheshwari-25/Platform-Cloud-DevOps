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
