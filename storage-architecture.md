---

copyright:
  years: 2024
lastupdated: "2024-11-12"

subcollection: pattern-pvs-ibmi-resiliency

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for storage
{: #storage-decisions}

| Architecture decision | Requirement | Alternatives | Decision | Rationale |
|------|------|------|-------|-----|
| Primary storage for local high availability workloads | Provide highly available storage for local high availability. | Flash Storage from IBM FS9500 series devices \n Tier 0 - 25 IOPS/GB \n Tier 1 - 10 IOPS/GB \n Tier 3 - 3 IOPS/GB \n Fixed5K IOPS | Matches production workload storage tier | LPARS share local storage |
| Primary storage for secondary site workloads | Provide highly available storage for disaster recovery workloads by using Global Replication Services  | Match production storage tiers | Match production storage Tiers | Global Replication Services (GRS) does not support mixed Tiers for the same environment. Storage needs to match like for like – Tier 1 to Tier 1, Tier 3 to Tier 3 |
| Backup Storage | Provide highly available storage for backups | Local Storage, Cloud Object Storage | Local storage + Cloud Object Storage | Local + Cloud Object Storage  | Local flashsystem storage is used for index and configuration repository. Cloud Object Storage is used for deduplication repository
| Long term and backup storage | Provide storage for long-term retention | Cloud Object Storage, Local Flashsystem Storage | Cloud Object storage| Archiving data is offloaded to Cloud Object Storage for long term retention  |
{: caption="Storage architecture decisions" caption-side="bottom"}
