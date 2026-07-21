# Module 02 - Administer Governance and Compliance

## Azure hierarchy and inheritance

```text
Root management group
  -> Management group(s)
    -> Subscription(s)
      -> Resource group(s)
        -> Resource(s)
```

RBAC assignments and Azure Policy assignments normally inherit from parent scopes. Design higher scopes for controls that must apply broadly and lower scopes for exceptions or workload-specific configuration.

## Subscriptions and resource groups

A subscription is a boundary for billing, quotas, resource providers, and access. A resource group is a logical container used to manage a set of resources as one lifecycle unit.

Remember:

- A resource belongs to one resource group at a time.
- A resource group has a location for its metadata; its resources can be in other supported regions.
- Deleting a resource group deletes the resources in it.
- Moves between resource groups/subscriptions are service-dependent and require validation.
- Quotas and service limits are often regional or subscription-scoped; request increases before deployment.
- Resource providers may need to be registered in the subscription.

## Tags

Tags are name/value metadata for organization, ownership, environment, application, and cost reporting.

- Tags do not automatically inherit by default.
- Azure Policy can add or inherit required tags.
- Tags are not a security boundary.
- Establish a controlled tag dictionary, for example `Environment`, `Owner`, `CostCenter`, and `DataClassification`.

## Resource locks

| Lock | Effect |
|---|---|
| CanNotDelete | Resource can be modified but not deleted |
| ReadOnly | Resource cannot be modified or deleted through the control plane |

Locks inherit to child resources. They protect against accidental control-plane changes, not data-plane operations. Remove or adjust a lock before legitimate maintenance that requires changes.

## Cost management

Use:

- Cost analysis for actual and forecasted spending.
- Budgets and alerts for thresholds.
- Tags and management-group/subscription structure for allocation.
- Azure Advisor recommendations for optimization.
- Reservations, savings plans, right-sizing, auto-shutdown, and lifecycle policies where suitable.

A budget alerts you; it does not normally stop resource consumption by itself.

## Azure Policy

Azure Policy evaluates resources against organizational rules.

Core objects:

- **Definition:** The rule and effect.
- **Assignment:** Applies a definition at a scope.
- **Parameter:** Makes a definition reusable.
- **Exemption:** Documents an approved exception.
- **Compliance state:** Compliant, non-compliant, exempt, conflicting, or not started.
- **Remediation task:** Corrects supported existing resources, commonly with `modify` or `deployIfNotExists`.

Common effects include `audit`, `deny`, `append`, `modify`, `deployIfNotExists`, and `disabled`. A new deny policy prevents future non-compliant deployments but does not automatically delete existing resources.

## Initiatives

An initiative is a collection of policy definitions assigned and reported as one unit. Use initiatives for baselines such as regional restrictions, mandatory diagnostic settings, approved SKUs, and required tags.

## Azure RBAC

An RBAC assignment combines:

```text
Security principal + Role definition + Scope
```

- **Security principal:** User, group, service principal, or managed identity.
- **Role definition:** Allowed control-plane and/or data-plane actions.
- **Scope:** Management group, subscription, resource group, or resource.

Use built-in roles first. Assign at the narrowest practical scope. `Owner` can manage access, `Contributor` cannot grant roles, and `Reader` is view-only for the control plane. Data access often requires a separate data-plane role.

## Azure RBAC vs Entra roles

| Azure RBAC | Microsoft Entra roles |
|---|---|
| Controls Azure resources | Controls directory and identity objects |
| Scopes include subscription/RG/resource | Scope is tenant or administrative unit, depending on role |
| Example: Virtual Machine Contributor | Example: User Administrator |

## Exam cues

- Use **Policy** to enforce what can be deployed; use **RBAC** to control who can act.
- A lock is not a substitute for RBAC or Policy.
- Higher-scope assignments inherit downward unless excluded or otherwise overridden by supported behavior.
- For unexpected access, inspect all inherited role assignments and deny assignments.
- For unexpected policy results, check assignment scope, exclusions, parameters, effect, and compliance evaluation time.
