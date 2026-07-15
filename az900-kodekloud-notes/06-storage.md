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

### VM attachment vs. storage service URLs

**Azure Managed Disks are the storage service in this list designed to be attached to a VM** as an operating-system disk or data disk. The VM reads and writes to the disk as block storage.

Blob, Files, Queue, and Table storage are accessed through storage-account service endpoints:

- Blob: `https://<storage-account>.blob.core.windows.net`
- Files: `https://<storage-account>.file.core.windows.net`
- Queue: `https://<storage-account>.queue.core.windows.net`
- Table: `https://<storage-account>.table.core.windows.net`

These services have public endpoint URLs by default, but that **does not mean the stored data is publicly accessible**. Authentication and authorization are still required unless anonymous access is deliberately enabled where supported. Public network access can also be restricted with storage firewalls or replaced with private-endpoint access.

> **Exam tip:** Managed Disk = attach block storage to a VM. Blob, Files, Queue, and Table = access through a storage service endpoint. Azure Files can also be mounted by a VM, but it remains a managed file share accessed through the Azure Files service.

## Redundancy options

| Option | Copies data across | Protects against |
|---|---|---|
| LRS | Multiple copies in one data center | Local disk/server failure |
| ZRS | Availability zones in one region | Data-center/zone failure |
| GRS | A primary region and a paired secondary region | Regional disaster |
| GZRS | Zones in the primary region plus a secondary region | Zone and regional failure |

Read-access variants allow reads from the secondary region before Microsoft initiates a failover. Greater redundancy normally costs more.

## Blob access tiers

| Tier | Availability | Access pattern | Recommended minimum retention | Cost pattern |
|---|---|---|---|---|
| Hot | Online; immediate access | Frequently read or modified | None | Highest storage cost, lowest access cost |
| Cool | Online; immediate access | Infrequently accessed | 30 days | Lower storage cost, higher access cost |
| Cold | Online; immediate access | Rarely accessed but must remain quickly available | 90 days | Lower storage cost than Cool, higher access cost |
| Archive | Offline; must be rehydrated | Almost never accessed and stored long term | 180 days | Lowest storage cost, highest retrieval cost |

### Hot tier

- Best for active data that applications read or update often.
- Example uses include website images, frequently downloaded documents, and current application data.
- You pay more to store the data, but reading and modifying it costs less than in cooler tiers.
- Choose Hot when frequent or unpredictable access makes retrieval charges important.

### Cool tier

- Best for data that is not used regularly but still needs immediate online access when requested.
- Example uses include short-term backups, older media, and reports accessed perhaps monthly.
- Storage is cheaper than Hot, but reading, writing, and other operations cost more.
- Data should normally remain in this tier for at least **30 days**.

### Cold tier

- Best for data that is accessed very rarely but must still be available immediately without rehydration.
- Example uses include long-term backups or records that might be read only a few times per year.
- Storage is cheaper than Cool, but retrieval and operation charges are higher.
- Data should normally remain in this tier for at least **90 days**.

### Archive tier

- Best for data that is almost never accessed and can remain offline.
- Example uses include compliance records, long-term backups, and original raw data retained for legal or historical reasons.
- Archived blobs cannot be read immediately. They must first be **rehydrated** to an online tier such as Hot, Cool, or Cold, which can take hours.
- Archive offers the lowest storage price but the highest retrieval cost and longest retrieval delay.
- Data should normally remain archived for at least **180 days**.

> **Cost rule:** moving, overwriting, or deleting a blob before its tier's minimum retention period ends can cause a prorated early-deletion charge. In general, moving toward colder tiers reduces storage cost but increases access cost and restrictions.

## Data migration services

### Azure Migrate

- Central hub for discovering, assessing, and migrating workloads to Azure.
- Helps estimate readiness, sizing, dependencies, and cost.
- Supports servers, databases, web apps, and virtual desktops through migration tools.

### Azure Data Box

- A physical device commonly used to transfer very large datasets **from an on-premises environment to Azure cloud storage** when network transfer is too slow or expensive.
- For an import, on-premises data is copied to the secured Data Box device, the device is shipped back to Microsoft, and Microsoft uploads the data into the selected Azure Storage account.
- Data Box also supports export scenarios that transfer data from Azure back to an on-premises environment.
- Useful for offline bulk migration or moving data from locations with limited connectivity.

#### Types of Azure Data Box

| Type | Form and capacity | Transfer direction | Best fit / status |
|---|---|---|---|
| **Data Box Disk** | Microsoft ships 1–5 encrypted SSDs; up to about 35 TB usable per order | Import to Azure only | Smaller offline transfers where portable disks are sufficient |
| **Data Box** | Rugged network-connected storage appliance; current next-generation models offer up to 120 TB or 525 TB usable, depending on the SKU | Import to or export from Azure | Large transfers in the tens or hundreds of terabytes |
| **Data Box Heavy** | Large wheeled appliance with roughly 1 PB raw capacity and about 770 TB usable | Import to or export from Azure | Historical option for extremely large transfers; **retired and no longer available to order** |

> **Current-service note:** the older 80-TB Data Box is being retired in regions as next-generation Data Box devices become available. Capacities and regional availability can therefore vary.

## File and data-management tools

| Tool | Type | Best use |
|---|---|---|
| AzCopy | Command-line utility | Scripted, high-performance copy to and from Azure Storage |
| Azure Storage Explorer | Desktop GUI | Browse and manage blobs, files, queues, and tables |
| Azure File Sync | Synchronization service | Cache and synchronize Azure Files with Windows Servers |

### AzCopy

- A fast **command-line tool** for copying data into, out of, or between Azure Storage locations.
- Can upload local files to Azure, download Azure data locally, and copy data between storage accounts.
- Useful for large or repeated transfers because commands can be placed in scripts and automation pipelines.
- Best when you are comfortable with a terminal and do not need a graphical interface.
- Example: use AzCopy to upload a large folder of backup files to a Blob Storage container.

### Azure Storage Explorer

- A free **desktop application with a graphical interface** for Windows, macOS, and Linux.
- Lets you visually browse storage accounts and manage blobs, file shares, queues, tables, and other supported storage resources.
- Common actions include creating containers, uploading or downloading files, viewing properties, and copying or deleting items.
- Best for administrators, developers, or learners who want to manage storage without writing commands.
- Example: use Storage Explorer to open a blob container and manually upload a few images.

### Azure File Sync

- A service that synchronizes an **Azure file share with one or more Windows Servers**.
- Users can continue working with files through the familiar local Windows file server while Azure Files keeps the central cloud copy.
- Changes made on one registered server are synchronized through the Azure file share to other registered servers.
- Optional **cloud tiering** keeps frequently used files cached locally and replaces less-used files with small placeholders, freeing local disk space. The full file is downloaded when needed.
- Best for extending or modernizing an existing on-premises Windows file server, multi-site file sharing, and centralized cloud storage.

> **Quick choice:** use **AzCopy** for fast or scripted copying, **Storage Explorer** for manual visual management, and **Azure File Sync** for ongoing synchronization between Windows Servers and Azure Files.

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
- [Microsoft Learn — Storage account endpoints](https://learn.microsoft.com/azure/storage/common/storage-account-overview#storage-account-endpoints)
- [Microsoft Learn — Azure managed disks overview](https://learn.microsoft.com/azure/virtual-machines/managed-disks-overview)
- [Microsoft Learn — Access tiers for blob data](https://learn.microsoft.com/azure/storage/blobs/access-tiers-overview)
- [Microsoft Learn — Blob rehydration from the Archive tier](https://learn.microsoft.com/azure/storage/blobs/archive-rehydrate-overview)
- [Microsoft Learn — Azure Data Box overview](https://learn.microsoft.com/azure/databox/data-box-overview)
- [Microsoft Learn — Azure Data Box Disk overview](https://learn.microsoft.com/azure/databox/data-box-disk-overview)
- [Microsoft Learn — Azure Data Box Heavy overview](https://learn.microsoft.com/azure/databox/data-box-heavy-overview)
- [Microsoft Learn — Get started with AzCopy](https://learn.microsoft.com/azure/storage/common/storage-use-azcopy-v10)
- [Microsoft Learn — Get started with Azure Storage Explorer](https://learn.microsoft.com/azure/storage/storage-explorer/vs-azure-tools-storage-manage-with-storage-explorer)
- [Microsoft Learn — Introduction to Azure File Sync](https://learn.microsoft.com/azure/storage/file-sync/file-sync-introduction)
