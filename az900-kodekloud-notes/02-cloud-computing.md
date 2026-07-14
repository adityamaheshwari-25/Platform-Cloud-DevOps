# Module 2 — Cloud Computing

## Cloud computing

Cloud computing delivers resources such as servers, storage, databases, and networking over the internet. Resources can be provisioned quickly and released when no longer needed.

## Deployment models

| Model | Meaning | Best fit |
|---|---|---|
| Public cloud | A cloud provider owns and operates shared infrastructure | Fast deployment, elastic scale, pay-as-you-go use |
| Private cloud | Infrastructure is dedicated to one organization | Maximum control or specialized compliance requirements |
| Hybrid cloud | Public and private/on-premises environments work together | Gradual migration, data-location needs, or temporary cloud capacity |

Hybrid is a model, not simply “some servers in two places.” The environments must be connected and managed as part of one solution.

## CapEx and OpEx

| CapEx | OpEx |
|---|---|
| Large upfront purchase | Ongoing payment for use |
| Own hardware and facilities | Rent provider-managed capacity |
| Capacity must be predicted early | Capacity can change with demand |
| Assets depreciate over time | Costs follow consumption more closely |

Cloud services commonly shift spending from **capital expenditure (CapEx)** toward **operational expenditure (OpEx)**.

## Consumption-based model

- Pay only for the resources consumed.
- Increase or decrease resources as demand changes.
- Stop or remove unused resources to avoid unnecessary cost.
- Cost is variable, so budgets and monitoring are important.

## Cloud benefits

| Benefit | Meaning |
|---|---|
| High availability | Keeps a service accessible despite failures or maintenance |
| Scalability | Adds or removes capacity to meet demand |
| Elasticity | Automatically or rapidly adjusts capacity as demand changes |
| Reliability | Recovers from failures and continues operating consistently |
| Predictability | Improves confidence in performance and cost planning |
| Security | Provides tools, controls, and large-scale provider investment |
| Governance | Applies organizational standards across resources |
| Manageability | Supports management through portals, APIs, command-line tools, and automation |

### Scaling directions

- **Vertical scaling (scale up/down):** change the CPU, memory, or size of one resource.
- **Horizontal scaling (scale out/in):** add or remove resource instances.

### Important distinction

- **Scalability** is the ability to change capacity.
- **Elasticity** is capacity changing dynamically with demand.

## Shared responsibility reminder

Moving to the cloud does not transfer every responsibility to Microsoft. The provider secures the cloud infrastructure; the customer still owns responsibilities such as data, identities, access, and configuration. The exact split depends on IaaS, PaaS, or SaaS.

## Exam traps

- Public cloud does not mean customer data is public.
- High availability reduces downtime; it does not promise that failures never occur.
- Scaling up changes one machine; scaling out changes the number of machines.
- Pay-as-you-go can reduce upfront cost, but unmanaged resources can still create a large bill.

## Quick check

1. Which model combines on-premises/private resources with public cloud? **Hybrid cloud.**
2. Buying physical servers is normally what type of expense? **CapEx.**
3. Adding more VM instances is which type of scaling? **Horizontal scaling.**
4. What benefit automatically matches capacity to demand? **Elasticity.**

## References

- [KodeKloud — Cloud Computing](https://notes.kodekloud.com/docs/Microsoft-Azure-Fundamentals-AZ-900/Cloud-Computing/Introduction)
- [KodeKloud — Cloud Models](https://notes.kodekloud.com/docs/Microsoft-Azure-Fundamentals-AZ-900/Cloud-Computing/Cloud-Models)
- [KodeKloud — Cloud Benefits](https://notes.kodekloud.com/docs/Microsoft-Azure-Fundamentals-AZ-900/Cloud-Computing/Cloud-Benefits-Part-1)

