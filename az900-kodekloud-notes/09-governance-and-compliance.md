# Module 9 — Governance and Compliance

## Governance controls compared

| Control | Main question | Example |
|---|---|---|
| Azure RBAC | Who can perform an action? | Developers can restart VMs in one resource group |
| Azure Policy | What configurations are allowed or required? | Only approved regions may be used |
| Resource lock | Which management changes must be prevented? | A production database cannot be deleted |

## Azure Policy

- Evaluates resources against organizational rules.
- Can audit, deny, modify, or deploy required configuration depending on the policy effect.
- A **policy definition** describes a rule.
- A **policy assignment** applies it to a scope.
- An **initiative** groups related policy definitions.
- The compliance view shows compliant and noncompliant resources.
- Assignments can inherit from management groups, subscriptions, and resource groups.

Examples:

- Require specific tags.
- Restrict allowed regions or VM sizes.
- Require diagnostic settings.
- Audit encryption or security configuration.

Policy does not replace RBAC. A user might have permission to deploy a resource, but Policy can still deny a noncompliant configuration.

## Resource locks

| Lock | Effect |
|---|---|
| Delete / CanNotDelete | Authorized users can modify the resource but cannot delete it until the lock is removed |
| ReadOnly | Authorized users can read the resource but cannot update or delete it |

- Locks are inherited by child resources.
- Locks protect control-plane operations, not all data-plane operations.
- A user with sufficient permission can remove a lock and then perform the action.
- Apply locks carefully because ReadOnly can interfere with operations that need to write configuration.

## Service Trust Portal

Microsoft’s portal for trust, privacy, compliance, and audit information.

It provides items such as:

- Audit reports
- Compliance certifications
- Regulatory documentation
- Security and privacy resources

Use it when an auditor or compliance team needs Microsoft’s assurance documentation.

## Microsoft Purview

Microsoft Purview provides data governance, risk, and compliance capabilities across supported cloud, on-premises, and SaaS data sources.

Capabilities include:

- Discovering and cataloging data
- Classifying sensitive information
- Tracking data lineage
- Supporting data governance and compliance
- Applying information-protection and data-lifecycle controls through the broader Purview family

## Exam traps

- RBAC grants permissions; Policy evaluates configuration.
- A resource lock applies even when a user has RBAC permission for the blocked operation.
- Policy initiatives are collections of policy definitions.
- Service Trust Portal contains Microsoft compliance evidence; Purview helps govern an organization’s data.

## Quick check

1. Need to restrict deployments to approved regions? **Azure Policy.**
2. Need to prevent accidental deletion? **A Delete resource lock.**
3. Need Microsoft audit reports? **Service Trust Portal.**
4. Need data discovery, classification, and lineage? **Microsoft Purview.**

## References

- [KodeKloud — Governance and Compliance](https://notes.kodekloud.com/docs/Microsoft-Azure-Fundamentals-AZ-900/Governance-and-Compliance/Introduction)
- [KodeKloud — Azure Policies](https://notes.kodekloud.com/docs/Microsoft-Azure-Fundamentals-AZ-900/Governance-and-Compliance/Azure-Policies)
- [KodeKloud — Microsoft Purview](https://notes.kodekloud.com/docs/Microsoft-Azure-Fundamentals-AZ-900/Governance-and-Compliance/Microsoft-Purview)

