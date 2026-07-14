# Module 8 — Cost Management

## Factors that affect Azure cost

- **Resource type and size:** larger or premium resources cost more.
- **Consumption:** running time, transactions, and stored data affect charges.
- **Region:** prices can differ by location.
- **Data transfer:** outbound data transfer can create charges; inbound transfer is often free.
- **Pricing model:** pay-as-you-go, reservations, savings options, and spot pricing have different trade-offs.
- **Licensing:** existing eligible licenses can reduce some costs through Azure Hybrid Benefit.
- **Support plan:** support level can add a recurring charge.

Always confirm current prices and eligibility in official calculators and documentation.

## Azure Marketplace

- Catalog of Microsoft and third-party solutions certified for Azure.
- Includes applications, virtual-machine images, security products, and managed services.
- A Marketplace purchase may include Azure infrastructure charges and separate publisher charges.

## Cost tools

| Tool | When used | Purpose |
|---|---|---|
| Pricing Calculator | Before deployment | Estimate the cost of a proposed Azure solution |
| TCO Calculator | Before migration | Compare estimated on-premises costs with Azure costs |
| Cost Management | During operation | Analyze actual spending, create budgets, forecast, and optimize |

### Pricing Calculator

- Select services, sizes, regions, usage, and support options.
- Produces an estimate, not a guaranteed invoice.
- Useful for comparing architecture options.

### Total Cost of Ownership (TCO) Calculator

- Models current on-premises infrastructure and operational costs.
- Estimates potential cost differences when moving workloads to Azure.
- Considers areas such as hardware, electricity, facilities, software, and staffing assumptions.

### Azure Cost Management

- Breaks down costs by subscription, resource group, service, location, or tag.
- Supports budgets and alerts.
- Provides forecasts and cost recommendations.
- Can export cost data for reporting.

A budget sends warnings; it does not normally shut down resources automatically.

## Resource tags

Tags are key-value metadata attached to Azure resources, resource groups, and subscriptions.

Examples:

```text
Environment = Production
Department  = Finance
CostCenter  = CC-1042
Owner       = Platform-Team
```

Tags help with organization, automation, reporting, and cost allocation. Tags assigned to a resource group do not automatically appear on each resource unless a policy or automation applies them.

## Cost-optimization habits

- Remove unused resources.
- Right-size compute and storage.
- Shut down nonproduction resources when not needed.
- Use reservations or savings options for stable workloads after reviewing commitment terms.
- Select suitable storage access tiers.
- Create budgets and alerts.
- Use tags consistently for cost ownership.
- Review Azure Advisor cost recommendations.

## Exam traps

- Pricing Calculator estimates a future Azure design; TCO Calculator compares on-premises and Azure.
- Cost Management works with actual deployed-resource spending.
- A budget alert is not a spending cap.
- Tags do not directly stop an action or grant permission.

## Quick check

1. Which tool estimates a proposed Azure architecture? **Pricing Calculator.**
2. Which tool compares on-premises costs with Azure? **TCO Calculator.**
3. Which tool analyzes actual Azure spending? **Azure Cost Management.**
4. Do resource-group tags automatically inherit to resources? **No.**

## References

- [KodeKloud — Cost Management](https://notes.kodekloud.com/docs/Microsoft-Azure-Fundamentals-AZ-900/Cost-Management/Introduction)
- [KodeKloud — Pricing Calculator](https://notes.kodekloud.com/docs/Microsoft-Azure-Fundamentals-AZ-900/Cost-Management/Pricing-Calculator)
- [KodeKloud — Resource Tags](https://notes.kodekloud.com/docs/Microsoft-Azure-Fundamentals-AZ-900/Cost-Management/Resource-Tags)

