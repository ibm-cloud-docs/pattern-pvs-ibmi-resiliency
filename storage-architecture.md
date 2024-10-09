---

copyright:
  years: 2024
lastupdated: "2024-06-20"

subcollection: pattern-pvs-ibmi-resiliency

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for storage
{: #storage-decisions}




| Architecture decision | Requirement | Alternatives | Decision | Rationale |
|------|------|------|-------|-----|
| Primary Storage for local HA workloads | Provide highly available storage for local high availability. | Flash Storage from IBM FS9000 series devices Tier 1 - 10 IOPS | Matches production workload storage tier | LPARS share local storage |
| Primary Storage for secondary site workloads | Provide highly available storage for disaster recovery workloads by using Global Replication Services  | Match production storage tiers | Match production storage Tiers | Global Replication Services (GRS) does not support mixed Tiers for the same environment. Storage needs to match like for like – Tier 1 to Tier 1, Tier 3 to Tier 3 |
| Backup Storage | Provide highly available storage for backups | Local Storage Cloud Object Storage Managed Service | Local storage + Cloud Object Storage Storage is deployed as part of the managed backup solution. | Local + Cloud Object Storage storage for mksysb images \n \n Storage is deployed as part of the managed backup solution. \n \n Secure Automated Backup with Compass is a fully managed backup solution for AIX and Linux workloads. \n \n Backup servers are preconfigured in datacenters and replicated to another region. \n \n 2 copies of data, one in each MZR, service is set up in pairs; validate Secure Automated Backup with Compass datacenter pairings match your workload locations. \n \n For sizing and configuration contact Cobalt Iron [support](https://www.cobaltiron.com/) https://cloud.ibm.com/catalog/services/secure-automated-backup-with-compass\#about |
| Long Term/Backup Storage | Provide storage for long-term retention | |Managed backup storage solution| Storage is deployed as part of the managed backup solution. |
{: caption="Storage architecture decisions" caption-side="bottom"}
