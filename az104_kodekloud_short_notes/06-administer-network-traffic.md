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
