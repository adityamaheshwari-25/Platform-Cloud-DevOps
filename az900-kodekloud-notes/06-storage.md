# Module 6 — Storage

## Storage accounts

An Azure storage account provides a unique namespace and access to Azure Storage services. Data is encrypted at rest and can be accessed securely over HTTPS.

Key design choices include:

- Region
- Performance tier
- Redundancy option
- Access tier
- Network and access controls

## Storage services

| Service | Stores | Common use |
|---|---|---|
| Blob Storage | Unstructured objects | Images, video, backups, logs, data lakes |
| Azure Files | Managed file shares using SMB or NFS | Shared files and replacing file servers |
| Queue Storage | Messages | Asynchronous communication between application components |
| Table Storage | NoSQL key/attribute data | Large, flexible, non-relational datasets |
| Managed Disks | Block storage | Persistent disks for Azure VMs |

## Redundancy options

| Option | Copies data across | Protects against |
|---|---|---|
| LRS | Multiple copies in one data center | Local disk/server failure |
| ZRS | Availability zones in one region | Data-center/zone failure |
| GRS | A primary region and a paired secondary region | Regional disaster |
| GZRS | Zones in the primary region plus a secondary region | Zone and regional failure |

Read-access variants allow reads from the secondary region before Microsoft initiates a failover. Greater redundancy normally costs more.

## Blob access tiers

| Tier | Access pattern | Cost pattern |
|---|---|---|
| Hot | Frequently accessed | Higher storage cost, lower access cost |
| Cool | Infrequently accessed | Lower storage cost, higher retrieval cost |
| Cold | Rarely accessed but still needs online access | Lower storage cost, higher access/early-deletion cost |
| Archive | Long-term data that can be offline | Lowest storage cost, highest retrieval cost and rehydration delay |

Access tiers apply to blob data. Minimum retention and early deletion rules depend on the selected tier and current Azure terms.

## Data migration services

### Azure Migrate

- Central hub for discovering, assessing, and migrating workloads to Azure.
- Helps estimate readiness, sizing, dependencies, and cost.
- Supports servers, databases, web apps, and virtual desktops through migration tools.

### Azure Data Box

- A physical device used to transfer very large datasets when network transfer is too slow or expensive.
- Data is copied locally, the secured device is shipped, and Microsoft uploads the data to Azure.
- Useful for offline bulk migration or moving data from locations with limited connectivity.

## File and data-management tools

| Tool | Type | Best use |
|---|---|---|
| AzCopy | Command-line utility | Scripted, high-performance copy to and from Azure Storage |
| Azure Storage Explorer | Desktop GUI | Browse and manage blobs, files, queues, and tables |
| Azure File Sync | Synchronization service | Cache and synchronize Azure Files with Windows Servers |

## Exam traps

- LRS has multiple copies, but they remain in one data-center location.
- ZRS protects against a zone failure; GRS protects against regional loss.
- Archive data is offline and must be rehydrated before normal access.
- Azure Migrate is an assessment/migration hub; Data Box is physical bulk transfer.
- Blob Storage is object storage; Azure Files provides file shares.

## Quick check

1. Which service stores images and backups as objects? **Blob Storage.**
2. Which redundancy option copies across availability zones in one region? **ZRS.**
3. Which tier is intended for long-term offline data? **Archive.**
4. Which tool is useful when petabytes cannot be transferred efficiently over the network? **Azure Data Box.**

## References

- [KodeKloud — Storage](https://notes.kodekloud.com/docs/Microsoft-Azure-Fundamentals-AZ-900/Storage/Introduction)
- [KodeKloud — Redundancy Options](https://notes.kodekloud.com/docs/Microsoft-Azure-Fundamentals-AZ-900/Storage/Redundancy-Options)
- [KodeKloud — File Management Options](https://notes.kodekloud.com/docs/Microsoft-Azure-Fundamentals-AZ-900/Storage/File-Management-Options)

