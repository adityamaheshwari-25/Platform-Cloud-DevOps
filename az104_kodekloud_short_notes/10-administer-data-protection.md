# Module 10 - Administer Data Protection

## Backup vs disaster recovery

| Azure Backup | Azure Site Recovery (ASR) |
|---|---|
| Creates recoverable restore points | Replicates workloads for failover |
| Protects against deletion, corruption, ransomware, and retention needs | Protects service continuity during site/region failure |
| Restore files, disks, VMs, or supported workloads | Fail over and later fail back/reprotect |
| RPO/RTO depends on backup schedule and restore process | RPO/RTO depends on continuous replication and recovery plan |

Many production systems need both.

## Vaults

- **Recovery Services vault:** Commonly manages Azure VM backup, MARS/MABS/DPM backup, and Azure Site Recovery.
- **Backup vault:** Used for newer Azure Backup workloads and capabilities. Supported data sources differ from a Recovery Services vault.

Choose the correct vault type, region, subscription, redundancy, security settings, and identity before enabling protection.

## Azure Backup flow

1. Create the appropriate vault.
2. Configure redundancy and security controls.
3. Create/select a backup policy with frequency and retention.
4. Enable backup for the data source.
5. Run or wait for the first backup.
6. Monitor jobs and alerts.
7. Test restore and document recovery steps.

A backup is only useful when restore has been tested.

## File and folder backup

The Microsoft Azure Recovery Services (MARS) agent can protect supported Windows files, folders, volumes, and system state directly to a Recovery Services vault.

- Install and register the agent with vault credentials.
- Set encryption/passphrase securely; Microsoft cannot recover a lost passphrase.
- Configure schedule, retention, bandwidth, and exclusions.
- Restore to the original or alternate location.

For larger server/application environments, Microsoft Azure Backup Server (MABS) or System Center DPM may be appropriate.

## Azure VM backup

Azure VM backup uses a VM extension and snapshots/transfer to create recovery points in the vault.

- Policy controls schedule and retention.
- Recovery points can be application-consistent, file-system-consistent, or crash-consistent depending on workload and conditions.
- Restore options can include a full VM, disks, or individual files.
- Ensure networking, encryption keys, identities, and dependencies are available during restore.

## Non-Azure VM backup

Options include:

- MARS for files/folders/system state on supported Windows machines.
- MABS/DPM for broader workloads.
- Azure Arc integration and service-specific solutions where supported.

Connectivity, proxy/firewall rules, credentials, and bandwidth are common failure points.

## Soft delete and security

Soft delete retains deleted backup data for a recovery period, helping defend against accidental or malicious deletion.

Also consider:

- Immutability where supported.
- Multi-user authorization for critical operations.
- Resource Guard and least-privilege backup roles.
- Alerts for disabled protection, deleted backup items, and failed jobs.
- Separate operational and security ownership for destructive actions.

## Azure Site Recovery

ASR replicates supported workloads to a recovery location.

Core sequence:

1. Define source and target resources.
2. Enable replication and select replication policy.
3. Map networks and configure target compute/storage settings.
4. Monitor replication health.
5. Run a **test failover** in an isolated network.
6. During an incident, perform planned or unplanned failover as appropriate.
7. Validate, commit, and then reprotect for failback/reverse replication.

Recovery plans can group machines, define order, and run automation steps.

## Exam cues

- Backup is retention/recovery; ASR is replication/failover.
- Know when to use a Recovery Services vault versus a Backup vault.
- Soft delete protects deleted backup data, not the original live resource by itself.
- Test failover should not disrupt production and should use an isolated network.
- Monitor backup jobs, reports, alerts, restore readiness, and ASR replication health.
