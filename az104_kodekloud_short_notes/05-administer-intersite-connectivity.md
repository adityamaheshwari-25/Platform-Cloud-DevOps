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
