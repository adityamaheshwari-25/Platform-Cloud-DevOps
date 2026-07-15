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

The Azure Pricing Calculator estimates what a **planned Azure solution** might cost before you deploy it.

- Add the Azure services you expect to use, such as virtual machines, storage, databases, and networking.
- Select details such as region, service tier, size, running hours, storage capacity, and expected usage.
- Review an estimated monthly cost and compare different architecture or sizing choices.
- Example: compare the estimated cost of two VM sizes or see how choosing another region changes the price.

The result is an estimate based on the information and prices selected; it is not a guaranteed invoice.

### Total Cost of Ownership (TCO) Calculator

The TCO Calculator is specifically used to estimate the financial effect of an **on-premises-to-Azure migration**.

- Enter details about the current on-premises environment, including servers, databases, storage, and networking.
- Include assumptions for costs such as hardware, electricity, facilities, software licensing, maintenance, and IT labor.
- Compare the estimated cost of continuing to run the workloads on-premises with the estimated cost of running them in Azure over a chosen period.
- Use the result to create an initial migration business case and identify possible savings.

The TCO Calculator estimates and compares costs; it does **not** perform the migration. Services such as Azure Migrate help assess and move the actual workloads.

### Azure Cost Management

Azure Cost Management helps monitor, analyze, and optimize **actual Azure spending after resources are deployed**.

- View costs by billing scope, subscription, resource group, resource, service, location, or tag.
- Use Cost Analysis to understand where money is being spent and identify cost trends.
- Create budgets and alerts that notify the appropriate people when actual or forecasted spending reaches a threshold.
- Review forecasts, cost-saving insights, and relevant Azure Advisor recommendations.
- Schedule cost-data exports to an Azure Storage account for reporting or further analysis.

A budget sends warnings; it does not normally shut down resources automatically.

> **Quick choice:** use **Pricing Calculator** to estimate a future Azure design, **TCO Calculator** to compare an on-premises-to-Azure migration, and **Cost Management** to analyze deployed-resource spending.

## Resource tags

Resource tags are simple **key-value labels** attached to Azure resources, resource groups, and subscriptions. A tag describes a resource without changing how that resource operates.

Examples:

```text
Environment = Production
Department  = Finance
CostCenter  = CC-1042
Owner       = Platform-Team
```

### Grouping resources with tags

Tags create a **logical grouping** of related resources. Resources with the same tag can be searched, filtered, or reported on together even when they are different resource types or are stored in different resource groups.

For example, a web app, database, storage account, and virtual machine might all belong to `Project = OnlineStore`. Filtering by this tag displays the project's resources together without requiring them to be in the same resource group.

A resource can have several tags at the same time:

```text
Project     = OnlineStore
Environment = Production
Department  = Sales
CostCenter  = CC-2050
Owner       = Web-Team
```

### Resource tags vs. resource groups

| Resource tags | Resource groups |
|---|---|
| Flexible labels used for logical grouping and reporting | Management containers that hold Azure resources |
| One resource can have multiple tags | A resource normally belongs to one resource group at a time |
| Can group related resources across resource groups | Usually groups resources with a related lifecycle |
| Do not control resource deletion or access by themselves | Can be a scope for RBAC, policies, locks, deployment, and deletion |

### Common uses

- **Cost allocation:** group spending by department, project, environment, or cost center in Cost Management.
- **Finding resources:** filter the Azure portal to locate all production resources or everything owned by a particular team.
- **Ownership:** record which team or person is responsible for a resource.
- **Automation:** scripts can find resources with a tag and perform actions such as starting or stopping development VMs.
- **Governance:** Azure Policy can require particular tags or add tags when resources are created.

### Important points

- Tags assigned to a resource group do **not** automatically appear on its resources; Azure Policy or automation can copy or require them. Cost Management tag inheritance can apply subscription or resource-group tags to cost records for reporting, but it does not modify the resources themselves.
- A tag does not grant permissions, enforce network security, or directly stop an action.
- Use a consistent naming standard, because variations such as `Environment = Prod` and `Environment = Production` can split reports into separate groups.
- Do not place passwords, secrets, or other sensitive information in tags.

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
- [Microsoft Azure — Pricing Calculator](https://azure.microsoft.com/pricing/calculator/)
- [Microsoft Azure — Total Cost of Ownership Calculator](https://azure.microsoft.com/pricing/tco/calculator/)
- [Microsoft Learn — Cost Management and Billing overview](https://learn.microsoft.com/azure/cost-management-billing/cost-management-billing-overview)
- [Microsoft Learn — Use tags to organize Azure resources](https://learn.microsoft.com/azure/azure-resource-manager/management/tag-resources)
- [Microsoft Learn — Define an Azure resource-tagging strategy](https://learn.microsoft.com/azure/cloud-adoption-framework/ready/azure-best-practices/resource-tagging)
