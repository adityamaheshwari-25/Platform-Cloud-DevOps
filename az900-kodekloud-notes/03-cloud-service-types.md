# Module 3 — Cloud Service Types

## IaaS, PaaS, and SaaS

| Type | You receive | You mainly manage | Azure example |
|---|---|---|---|
| IaaS | Virtualized compute, storage, and networking | OS, patches, applications, data, and configuration | Azure Virtual Machines |
| PaaS | A managed application platform and runtime | Application code, data, identities, and access | Azure App Service |
| SaaS | A complete, provider-managed application | Users, data, access, and configuration | Microsoft 365 |

The progression is:

**IaaS → more control and more customer management**  
**SaaS → less infrastructure control and less customer management**

## Infrastructure as a Service (IaaS)

- Closest cloud model to a traditional data center.
- Useful when an application needs OS-level control or custom software.
- Microsoft manages physical hosts, physical networking, and the data center.
- The customer manages the guest OS, patching, installed applications, and data.

## Platform as a Service (PaaS)

- Microsoft manages the infrastructure, OS, runtime, patching, and much of the scaling platform.
- Developers focus on deploying and maintaining application code and data.
- Useful for web applications and APIs that do not require OS-level control.

## Software as a Service (SaaS)

- A ready-to-use application delivered over the internet.
- The provider operates nearly the entire stack.
- The customer still manages users, access, data, and configuration.

## Shared responsibility

| Responsibility | On-premises | IaaS | PaaS | SaaS |
|---|---:|---:|---:|---:|
| Physical data center | Customer | Microsoft | Microsoft | Microsoft |
| Physical network and hosts | Customer | Microsoft | Microsoft | Microsoft |
| Operating system | Customer | Customer | Microsoft | Microsoft |
| Application/runtime | Customer | Customer | Shared/customer app | Microsoft |
| Identities and access | Customer | Customer | Customer | Customer |
| Data | Customer | Customer | Customer | Customer |

Some responsibilities are shared in practice, but the customer always retains responsibility for its data, accounts, and appropriate access controls.

## Choosing a model

- Need full guest-OS control? Choose **IaaS**.
- Need to deploy code without managing an OS? Choose **PaaS**.
- Need a finished business application? Choose **SaaS**.

## Exam traps

- A virtual machine is IaaS even though Microsoft manages its physical server.
- PaaS removes OS management, not responsibility for application code or data.
- SaaS does not remove the customer’s responsibility to manage identities and data access.

## Quick check

1. Which model provides the most customer control? **IaaS.**
2. Which model is best for focusing on web code instead of OS patching? **PaaS.**
3. Who owns customer data in every service model? **The customer.**

## References

- [KodeKloud — Cloud Service Types](https://notes.kodekloud.com/docs/Microsoft-Azure-Fundamentals-AZ-900/Cloud-Service-Types/Introduction)
- [KodeKloud — Shared Responsibility Model](https://notes.kodekloud.com/docs/Microsoft-Azure-Fundamentals-AZ-900/Cloud-Service-Types/Shared-Responsibility-Model)

