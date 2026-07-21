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
