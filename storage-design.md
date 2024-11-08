---

copyright:
  years: 2024
lastupdated: "2024-11-08"

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

When designing a FalconStor Virtual Tape Library (VTL) on IBM Cloud, it's essential to integrate several critical elements to ensure the system's efficiency, security, and scalability. Data deduplication is paramount as it significantly reduces storage requirements by eliminating duplicate data, thereby saving space and enhancing backup and restore performance. Performance is also a crucial factor, necessitating high-speed backup and restore operations to minimize downtime and ensure data availability. A well-thought-out data archival strategy, utilizing supported S3 cloud solutions, is vital for managing the data lifecycle effectively. Ransomware protection through air-gapped, immutable backups is necessary to safeguard data from malicious attacks. Moreover, enterprise-wide replication ensures data availability and robust disaster recovery across multiple sites.

Scalability should be a primary design consideration, allowing the system to grow from small-scale deployments to petabyte-scale environments seamlessly. Compatibility with existing backup software and hardware systems ensures smooth integration and operation. Offsite protection, achieved by exporting virtual tapes to physical tapes, enhances data security and compliance with offsite storage requirements. Effective management and monitoring are critical for maintaining system health and performance, with management dashboards offering real-time monitoring and efficient oversight of both virtual and physical tape libraries. This comprehensive approach ensures a reliable, secure, and scalable VTL solution on IBM Cloud.

## Storage considerations for high availability
{: #storage-ha-considerations}

The high availability method that is chosen for this pattern is PowerHA SystemMirror Standard Edition for Local HA Cluster. This method meets the requirements of local clustering. Storage considerations for this solution include several high availability (HA) factors.

To set up and manage a Cluster Aware repository on IBM i using PowerHA SystemMirror, you need to ensure all cluster nodes have access to the repository disk via SAN or SAS, configure the cluster through PowerHA interfaces, set up redundant communication paths for robustness, and leverage the Automatic Repository Disk Replacement (ARR) feature for continuous availability. Additionally, it's important to monitor cluster health using PowerHA interfaces and perform regular maintenance tasks to ensure smooth operations.

## Storage considerations for disaster recovery
{: #dr-storage-considerations}

The disaster recovery method that is chosen for this pattern is Global Replication Services (GRS). The storage considerations for this pattern concern the controller LPARs and the types of disaster recovery workloads

Each GRS control LPAR requires 300 GB of Tier 1 storage in both the primary and secondary regions. Global Replication Services (GRS) does not support mixed tiers within the same environment. Also, the storage in the disaster recovery site matches the tier that is deployed for the primary workload. For instance, if the primary site workload uses Tier 1 storage, the secondary site workload storage should also be Tier 1. Similarly, if the primary workload is on Tier 3 storage, the secondary site workload storage might align with Tier 3.
