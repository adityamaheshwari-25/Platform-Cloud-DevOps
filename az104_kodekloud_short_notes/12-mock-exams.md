# Module 12 - Mock Exams and Final Review

## Purpose of mock exams

Mock exams are diagnostic tools. The goal is not to memorize an answer key; it is to identify weak decision rules, missed constraints, and slow topics.

## Three-pass approach

### Pass 1 - Timed attempt

- Use exam-like timing and no notes.
- Mark uncertain questions instead of repeatedly rereading them.
- Record confidence: high, medium, or low.

### Pass 2 - Error analysis

For every wrong or low-confidence question, classify the cause:

| Cause | Corrective action |
|---|---|
| Concept gap | Re-read the relevant module and official docs |
| Service-selection gap | Build a comparison table and one scenario |
| Scope/inheritance mistake | Draw tenant -> MG -> subscription -> RG -> resource |
| Portal/command gap | Repeat the lab from memory |
| Misread requirement | Highlight words such as private, global, zone, least privilege, minimize cost |
| Outdated detail | Verify against current Microsoft documentation |

### Pass 3 - Rebuild and retest

- Recreate the weak scenario in a lab.
- Explain the solution aloud without looking at notes.
- Retake only after you can state why each alternative is wrong.

## High-value comparison list

Be able to choose quickly between:

- Entra role vs Azure RBAC role.
- RBAC vs Azure Policy vs resource lock.
- Service endpoint vs private endpoint.
- NSG vs UDR vs Azure Firewall/NVA.
- VNet peering vs VPN Gateway vs ExpressRoute.
- Load Balancer vs Application Gateway vs Front Door vs Traffic Manager.
- Availability set vs availability zone vs VMSS.
- App Service vs ACI vs Container Apps vs VM.
- Access key vs SAS vs Entra ID/RBAC.
- LRS vs ZRS vs GRS/GZRS.
- Azure Backup vs Site Recovery.
- Activity Log vs resource logs vs metrics.
- Alert rule vs action group vs alert processing rule.

## Final hands-on checklist

From a blank subscription, practice this sequence:

1. Create a resource group and apply tags.
2. Create users/groups and assign a least-privilege role.
3. Apply a policy and a delete lock.
4. Deploy a VNet with subnets, NSGs, and private DNS.
5. Deploy a VM without direct public management ports; connect through Bastion.
6. Create a storage account with private access, lifecycle, soft delete, and Entra authorization.
7. Deploy an App Service or container workload with managed identity.
8. Configure backup and perform a test restore.
9. Send logs to Log Analytics, run KQL, and create an alert/action group.
10. Export or recreate part of the environment with Bicep.

## Exam technique

- Identify the required outcome first, then eliminate services that fail a hard constraint.
- Watch for scope and inheritance.
- Prefer least privilege and private access.
- Do not add a new service when an existing configuration change meets the requirement.
- Distinguish high availability, backup, and disaster recovery.
- Recheck whether a question asks for the lowest cost, least administration, fastest recovery, or strongest isolation.

## Readiness signal

You are close to ready when you can consistently explain not only the correct answer, but also why each plausible alternative fails one requirement.
