---

copyright:
  years: 2024
lastupdated: "2024-11-12"

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

When designing a FalconStor Virtual Tape Library (VTL) on {{site.data.keyword.Bluemix_notm}}, it's essential to integrate several critical elements to help ensure the system's efficiency, security, and scalability. Data deduplication is paramount as it significantly reduces storage requirements by eliminating duplicate data, thereby saving space and enhancing backup and restore performance. Performance is also a crucial factor, needing high-speed backup and restore operations to minimize downtime and ensure data availability. A thought out data archival strategy, by using supported S3 cloud solutions, is vital for managing the data lifecycle effectively. Ransomware protection through air-gapped, immutable backups is necessary to safeguard data from malicious attacks. Enterprise-wide replication helps ensure data availability and robust disaster recovery across multiple sites.

Scalability should be a primary design consideration, allowing the system to grow from small-scale deployments to petabyte-scale environments seamlessly. Compatibility with existing backup software and hardware systems helps ensure smooth integration and operation. Offsite protection, which is achieved by exporting virtual tapes to physical tapes, enhances data security and compliance with offsite storage requirements. Effective management and monitoring are critical for maintaining system health and performance, with management dashboards offering real-time monitoring and efficient oversight of both virtual and physical tape libraries. This comprehensive approach helps ensure a reliable, secure, and scalable VTL solution on IBM Cloud.

## Storage considerations for high availability
{: #storage-ha-considerations}

IBM PowerHA SystemMirror for i is selected for its effectiveness in meeting the requirements of local clustering. The solution's storage considerations encompass several high availability (HA) factors to help ensure robust data protection and system resilience.

Synchronous Geographic Mirroring within PowerHA provides real-time data replication between an independent disk pool at the production system and a second remote system. This guarantees that both copies of the data are identical, offering zero data loss during failures. However, this requires the production and mirrored sites to be close due to the real-time replication needs.

This mirroring solution is ideal for organizations with internal storage needing protection against both planned and unplanned outages, without the necessity for site disaster protection. It works with any storage type, helping ensure that data copies are identical (RPO). Limitations include the need for sufficient bandwidth to support maximum production write rates and the need for full synchronization upon abnormal vary-off, during which no valid second copy is available.

## Storage considerations for disaster recovery
{: #dr-storage-considerations}

The disaster recovery method that is chosen for this pattern is Global Replication Services (GRS). The storage considerations for this pattern concern the controller LPARs and the types of disaster recovery workloads.

Each GRS control LPAR requires 300 GB of Tier 1 storage in both the primary and secondary regions. Global Replication Services (GRS) does not support mixed tiers within the same environment. Also, the storage in the disaster recovery site matches the tier that is deployed for the primary workload. For instance, if the primary site workload uses Tier 1 storage, the secondary site workload storage should also be Tier 1. Similarly, if the primary workload is on Tier 3 storage, the secondary site workload storage might align with Tier 3.
