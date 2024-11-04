---

copyright:
  years: 2024
lastupdated: "2024-11-04"

subcollection: pattern-pvs-ibmi-resiliency

keywords:

---

# Architecture decisions for resiliency
{: #resiliency-architecture}

The following sections summarize the resiliency architecture decisions for workloads deployed on {{site.data.keyword.vpc_short}} infrastructure.

| Architecture decision | Requirement | Alternatives | Decision | Rationale |
|------|-------|-------|-------|-----|
| Backups | Backup AIX workloads with a managed service | Image capture snapshots and FlashCopy \n \n Secure Automated Backup with Compass \n \n Veeam \n \n {{site.data.keyword.IBM_notm}} Storage Protect \n \n Falconstor VTL \n \n Bring Your Own Backup | FalconStor VTL | FalconStor Virtual Tape Library (VTL) is an optimized backup and deduplication solution that provides tape library emulation, high-speed backup or restore, data archival to supported S3 clouds for long-term storage, global data deduplication, enterprise-wide replication, and long-term cloud-based container archive, without requiring changes to the existing environment. |
| High availability | Local OS level high availability | Different server placement group \n \n PowerHA standard edition | PowerHA standard edition | Local availability optimization by allowing for the dynamic reconfiguration of running clusters. \n \n Minimize unscheduled downtime in response to unplanned cluster component failures. |
| Disaster recovery {{site.data.keyword.powerSys_notm}} workloads                      | Secondary data center with SAN-to-SAN replication  | Global Replication Services (GRS) and {{site.data.keyword.IBM_notm}} toolkit for AIX full system replication                                                              | Global Replication Services (GRS) and AIX toolkit for AIX full system replication  | Disaster recovery capability for RPO \< 1 hours, RTO \< 1 hours. \n \n {{site.data.keyword.IBM_notm}} toolkit for AIX from technology services enables automated disaster recovery functions and capabilities on the {{site.data.keyword.cloud_notm}} by integrating {{site.data.keyword.powerSys_notm}} with the capabilities of GRS. |
| Disaster recovery {{site.data.keyword.powerSys_notm}} workloads: Cost optimization | | Implement a shared processor pool \n Disaster recovery and nonproduction systems to share infrastructure. | Implement a shared processor pool | Set up shared processor pool to reserve capacity in the secondary region. Set up disaster recovery systems on minimum sized VMs to save operating cost.                                                                               |
{: caption="Resiliency architecture decisions" caption-side="bottom"}
