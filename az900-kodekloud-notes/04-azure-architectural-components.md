# Module 4 — Azure Architectural Components

## Regions and availability zones

- An **Azure region** is a geographical area containing one or more nearby data centers.
- An **availability zone** is a physically separate location within a region, with independent power, cooling, and networking.
- Deploying across zones protects a workload from a data-center-level failure.
- A **region pair** supports regional disaster-recovery planning for many Azure services.
- **Sovereign regions** serve specific legal or government requirements, such as Azure Government and Azure operated by 21Vianet in China.

Service availability, pricing, and supported zones vary by region.

## Azure resource hierarchy

| Level | Purpose |
|---|---|
| Management group | Organizes multiple subscriptions and applies governance above them |
| Subscription | Billing and access-management boundary |
| Resource group | Logical container for related resources |
| Resource | An individual service instance, such as a VM or storage account |

Hierarchy:

`Management group → Subscription → Resource group → Resource`

## Subscriptions

- Provide a billing boundary.
- Provide a scope for access control and policy.
- An organization can use multiple subscriptions to separate teams, environments, departments, or budgets.
- A subscription can belong to one management group at a time.

## Resource groups

- Every Azure resource belongs to one resource group.
- A resource group can contain different resource types and resources from different regions.
- Resources can usually be moved between resource groups when the service supports the move.
- Deleting a resource group deletes the resources inside it.
- Permissions and policies assigned at resource-group scope can be inherited by its resources.

Group resources by shared lifecycle when practical—for example, resources that are deployed, updated, and deleted together.

## Management groups

- Organize subscriptions into a hierarchy.
- Allow Azure Policy and RBAC assignments to flow down to subscriptions and resources.
- Useful for enterprise-wide governance.

## Availability design

| Failure to handle | Common design |
|---|---|
| Single VM or hardware host | Multiple instances / availability set |
| Single data center | Multiple availability zones |
| Entire region | Multi-region design and recovery plan |

## Exam traps

- Availability zones exist inside a region; region pairs involve separate regions.
- A resource group is a logical container, not a physical data center.
- A resource group’s location stores its metadata; resources inside it can be in other supported regions.
- Deleting a resource group is destructive to all resources it contains.

## Quick check

1. What is Azure’s billing boundary? **A subscription.**
2. What protects against a data-center-level failure within one region? **Availability zones.**
3. What sits above subscriptions in the hierarchy? **Management groups.**

## References

- [KodeKloud — Regions and Availability Zones](https://notes.kodekloud.com/docs/Microsoft-Azure-Fundamentals-AZ-900/Azure-Architectural-Components/Regions-and-Availability-Zones)
- [KodeKloud — Subscriptions and Resource Groups](https://notes.kodekloud.com/docs/Microsoft-Azure-Fundamentals-AZ-900/Azure-Architectural-Components/Subscriptions-and-Resource-Groups)

