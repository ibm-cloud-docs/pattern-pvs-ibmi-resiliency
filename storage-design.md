---

copyright:
  years: 2024
lastupdated: "2024-11-07"

subcollection: pattern-pvs-ibmi-resiliency

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Storage design
{: #storage-design}

Helping ensure storage resiliency for {{site.data.keyword.powerSysFull}} for IBM i involves having sufficient storage capacity to accommodate high availability, backup, and disaster recovery workloads. Users must carefully consider and understand the storage types and requirements that need to be met.

The requirements for the compute aspect for the Resiliency for PowerVS for IBM i workloads pattern focus on the following:

- The storage required to support backup activities.

- The storage required to support replication activities for disaster recovery.


## Storage considerations for backups
{: #storage-considerations}

The backup method that is chosen for this pattern is Secure Automated Backup with Compass and Make System Backup (MKSYSB). When sizing and configuring this method, it’s important to take several storage considerations into account.

Compass backup servers are preconfigured in data centers and are also replicated across other regions. The service is set up in pairs, with two copies of data—one in each multi-zone region (MZR). Validate that Secure Automated Backup with Compass is offered in the data center where your workloads are deployed. Consider storage size, including workload and data volumes that are both structured and unstructured, change and growth rates, and retention policies. For sizing guidance, see [Engage Cobalt Iron for sizing](https://cloud.ibm.com/catalog/services/secure-automated-backup-with-compass\#about){: external}. Help ensure that the file system has sufficient space to hold the produced mksysb image. Generally, 10 GB to 15 GB is sufficient, depending on any additional non-AIX data added to the rootvg. You might also want to consider Cloud Object Storage for storing mksysb images. 

## Storage considerations for high availability
{: #storage-ha-considerations}

The high availability method that is chosen for this pattern is PowerHA SystemMirror Standard Edition for Local HA Cluster. This method meets the requirements of local clustering. Storage considerations for this solution include several high availability (HA) factors.

In PowerHA SystemMirror, help ensure that the clustered workload LPARs share the volume group. The storage tier and size might be determined based on the workload requirements. Also, make sure that the Storage Logical Unit Number (LUN) for the Cluster Aware AIX (CAA) repository is set to the recommended 1 GB.

## Storage considerations for disaster recovery
{: #dr-storage-considerations}

The disaster recovery method that is chosen for this pattern is Global Replication Services (GRS). The storage considerations for this pattern concern the controller LPARs and the types of disaster recovery workloads

Each GRS control LPAR requires 300 GB of Tier 1 storage in both the primary and secondary regions. Global Replication Services (GRS) does not support mixed tiers within the same environment. Also, the storage in the disaster recovery site matches the tier that is deployed for the primary workload. For instance, if the primary site workload uses Tier 1 storage, the secondary site workload storage should also be Tier 1. Similarly, if the primary workload is on Tier 3 storage, the secondary site workload storage might align with Tier 3.
