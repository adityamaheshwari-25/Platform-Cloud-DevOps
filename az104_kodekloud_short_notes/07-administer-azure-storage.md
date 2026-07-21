# Module 07 - Administer Azure Storage

## Storage account basics

A storage account is the namespace and management boundary for Azure Storage services.

Main services:

- **Blob:** Unstructured object data.
- **Files:** Managed SMB/NFS file shares.
- **Queue:** Asynchronous messages.
- **Table:** NoSQL key/attribute data.
- **Disks:** Managed block storage for Azure VMs.

General-purpose v2 (`StorageV2`) is the default account type for most standard workloads. Premium account types target specific high-performance services.

## Redundancy

| Option | Copies / scope | Main use |
|---|---|---|
| LRS | Multiple copies in one datacenter | Lowest cost, local durability |
| ZRS | Synchronous copies across zones in one region | Zone resilience |
| GRS | LRS in primary plus asynchronous secondary region | Regional disaster protection |
| RA-GRS | GRS plus read access to secondary | Read from secondary during normal operation |
| GZRS | ZRS primary plus asynchronous secondary region | Zone and regional protection |
| RA-GZRS | GZRS plus read access to secondary | Highest read-access option among these |

Geo-replication is asynchronous, so the recovery point can lag. Choose redundancy based on RPO/RTO, regional support, cost, and workload requirements.

## Storage endpoints and network security

A storage account has service endpoints such as blob and file endpoints.

Security controls:

- Disable anonymous blob access unless explicitly required.
- Require secure transfer and a modern minimum TLS version.
- Use the storage firewall to allow selected networks/IPs.
- Use service endpoints for subnet-restricted access to the public endpoint.
- Use private endpoints for private IP connectivity and private DNS integration.
- Disable public network access when private-only access is required.

## Blob containers and blob types

A blob container groups blobs within a storage account.

- **Block blob:** Documents, media, backups, and general objects.
- **Append blob:** Append-heavy logging scenarios.
- **Page blob:** Random read/write pages; used by VHD-style workloads.

Container access should normally be private. Authorization should be explicit.

## Access tiers and lifecycle management

- **Hot:** Frequent access; higher storage cost, lower access cost.
- **Cool / Cold:** Infrequent access with minimum retention and higher access cost.
- **Archive:** Offline tier with low storage cost and rehydration delay.

A lifecycle policy can move blobs between tiers or delete old versions/snapshots based on age, prefix, tags, and other supported filters. Test policies carefully because deletion can be irreversible after retention features expire.

## Azure Files

Azure Files provides managed file shares over SMB and, for supported configurations, NFS.

Know how to:

- Create a share and set quota/tier.
- Mount from Windows or Linux.
- Use snapshots and soft delete.
- Configure identity-based access where required.
- Use Azure File Sync to cache an Azure file share on Windows Server.

## Encryption

- Storage Service Encryption encrypts data at rest by default.
- Keys can be Microsoft-managed or customer-managed through Key Vault where supported.
- Infrastructure encryption can add a second encryption layer for supported services.
- Azure Disk Encryption uses BitLocker or dm-crypt inside supported VMs; encryption at host protects data at the host layer. These are different controls.

## Authorization choices

| Method | Guidance |
|---|---|
| Microsoft Entra ID + Azure RBAC | Preferred for identity-based, least-privilege access |
| User delegation SAS | Delegated temporary access authorized through Entra ID |
| Service/account SAS | Scoped and time-limited access; protect the token |
| Account access keys | Broad power; rotate and avoid application embedding |

A SAS should specify only the required service/resource, permissions, start/expiry time, protocol, and optional IP range. A stored access policy can help manage service SAS behavior for supported services.

## Data-management tools

- Azure portal and Storage browser.
- Azure Storage Explorer.
- AzCopy for high-performance transfer.
- Azure CLI and PowerShell.

```bash
azcopy copy './data/*' 'https://ACCOUNT.blob.core.windows.net/CONTAINER?<SAS>' --recursive
```

## Exam cues

- Redundancy protects availability/durability; backup protects against logical deletion and retention requirements.
- Prefer Entra ID and RBAC over access keys.
- Service endpoint = public endpoint restricted to VNet; private endpoint = private IP.
- Know soft delete, versioning, snapshots, lifecycle management, object replication, and access tiers.
- For access failure, check identity role, SAS validity, firewall/private endpoint, DNS, protocol, and storage service path.
