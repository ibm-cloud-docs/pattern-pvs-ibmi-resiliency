---

copyright:
  years: 2024
lastupdated: "2024-11-12"

subcollection: pattern-pvs-ibmi-resiliency

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Resiliency design
{: #resiliency-design}

Resiliency is the ability for a workload to meet a specific target Service Level Objective (SLO), Service Level Availability (SLA), or recover from a service disruption and still meet the required SLA. Resiliency needs to be considered at both the infrastructure and application layers across the entire solution.

The following are requirements for the resiliency aspect for the {{site.data.keyword.powerSys_notm}} on IBM i pattern:

- A backup of all essential IBM i data.
- Provide the operating system level local high availability.
- Replicate {{site.data.keyword.powerSys_notm}} workloads from a protected site to a recovery site in a different region to enable the failover of workloads if there is a failure in the protected site.
- Failover that meets the required Recovery Time Objectives (RTO) and Recovery Point Objectives (RPO) of the application.

Resiliency needs to be considered for both the infrastructure and application levels. This pattern does not address application or database-specific design.
{: note}

It's important to validate what offerings are available in the regions you are deploying. Check for paired data centers and validate if they meet the deployment criteria for client-specific requirements.

## Resiliency design considerations for backup
{: #backup-considerations}

FalconStor Virtual Tape Library (VTL) is an optimized backup and deduplication solution that provides tape library emulation, high-speed backup or restore, data archival to supported S3 clouds for long-term storage, global data deduplication, enterprise-wide replication, and long-term cloud-based container archive, without requiring changes to the existing environment. The backup offering VPC and the {{site.data.keyword.powerSys_notm}} workspaces should be in the same region and connected by using the local Transit Gateway. For more Information, see [Managing a Virtual Tape Library](/docs/power-iaas?topic=power-iaas-manage-vtl).

Review key considerations and benefits when deploying the Falconstor VTL solution:

- File-level backup and restore
- Image-level backup and restore
- Policy management down to the directory and file object or type levels
- Backup and archive features that include long-term retention of data
- Backup and restore tool for periodic copies of data and applications to a separate, secondary device or secondary site and then by using those copies to recover the data and applications.
- Up to 95% data reduction. FalconStor reduces the storage capacity that is required by backup data on-premises and in the cloud by up to 95%.
- Ransomware protection to recover data from any point in time with air-gapped, immutable backups
- StorSafe VTL exports virtual tapes as physical tapes, and tracks their location with a management dashboard for offsite protection. Virtual tapes can remain in the library for fast restore or can be stubbed when exported to local object storage.
- High performance to improve both backup and recovery
- Modernize by using FalconStor that's 100% compatible with existing backup software, hardware, and operational procedures.

This third-party product is provided by a vendor outside of {{site.data.keyword.cloud_notm}} and is subject to a separate agreement between you and the third party if you accept their terms. {{site.data.keyword.cloud_notm}} is not responsible for the product and makes no privacy, security, performance, support, or other commitments regarding the product.
{: important}

For sizing and configuration information, contact [FalconStor VTL Support](https://www.falconstor.com/support/technical-support){: external}. For more information about the methods, see [FalconStor VTL summary](https://cloud.ibm.com/catalog/content/vtltile-tags-v10.03-01-f1e88e51-7e3d-4fbc-a7ed-3ab9adb2afea-global#about){: external}. The Falconstor sizing calculator can be found by going to [IBM Power Sizing Calculator](https://www.falconstor.com/sizing-calculator/?source=ibm){: external}. For additional information, review the [falconstor deployment guide](https://falconstor-download.s3.us-east.cloud-object-storage.appdomain.cloud/FalconStor%20VTL%20for%20IBM%20Deployment%20Guide.pdf){: external}.

## Resiliency design considerations for high availability
{: #ha-considerations}

The local operating system high availability method is IBM PowerHA SystemMirror for i.

By default, {{site.data.keyword.powerSys_notm}}s are restarted on a different host system if a hardware failure occurs. IBM PowerHA SystemMirror for i provides local clustering for mission-critical workloads. The clustering infrastructure allows you to create and manage multiple systems and system resources as a unified entity. Shared resources enable the cluster to continuously provide essential services to users and applications. Key PowerHA Cluster Functions include heartbeat monitoring for failure detection, activation and release of highly available services IPs, automatic activation of geographically mirrored volume groups, and start/stop/monitor of applications. Failover resource groups can move between cluster members and sites. For more information on PowerHA, see [high availability and disaster recovery](/docs/power-iaas?topic=power-iaas-ha-dr).

The following figure shows a configuration that uses IBM PowerHA SystemMirror for i using geographic mirroring.

![Standard PHA](/images/standardpha.svg "PowerHA geographic mirroring diagram"){: caption="Figure 2: PowerHA geographic mirroring architecture" caption-side="bottom"}{: external download="standardpha.svg"}

- Geographic mirroring refers to the IBM i host-based replication solution that is provided as a function of IBM PowerHA SystemMirror for i. 
- Geographic mirroring is managed by IBM i storage management so that replication is performed on a disk page segment basis. When a page of data is written, storage management automatically manages the replication of this page to the remote system.
- Geographic mirroring requires a two-node clustered environment and uses data port services. These data port services are provided by the System Licensed Internal Code (SLIC) to support the transfer of large volumes of data between a source node and a target node.

## Resiliency design considerations for disaster recovery
{: #dr-design}

The Power Systems Virtual Server service provides a Tier 2 99.95% SLA by default. When a Logical Partition (LPAR) has an outage within the service, it automatically attempts to restart that LPAR on a separate host. For a Tier 3 SLA of 99.99%, the workload is distributed across two data centers. Supporting a 1 hour RTO and 1 hour RPO, the solution that is described in this pattern includes:

- A secondary data center
- SAN to SAN replication paired between two {{site.data.keyword.IBM_notm}} {{site.data.keyword.powerSys_notm}} (PowerVS) workspaces in different data centers.
- Global Replication services (GRS) are based on industry standards {{site.data.keyword.IBM_notm}} Storwize Global Mirror Change Volume Asynchronous replication technology. For more information, see [getting started with GRS](/docs/power-iaas?topic=power-iaas-getting-started-GRS).

The GRS involves two sites, over 300 km apart, where storage replication is enabled. These two sites are fixed and mapped into a one-to-one relationship mode in both directions. These two sites are fixed and are in replication partnership in both directions. You can create a replication-enabled volume from any site, the site from where the request is initiated contains the main volume and play the role of primary. The remote site is auxiliary and contains an auxiliary volume.

Review the key capabilities for disaster recovery design considerations:

- Volume-based async storage replication that uses Consistency Groups (CG).
- CG Services to create and delete CGs, add and remove volumes, stop and start, and so on.
- Volume services to create, delete, and retype enabled to support mirrored volumes.
- Virtual machines services to deploy, delete, attach, detach, clone, and snapshot for mirrored volumes.

When a write operation is issued to a source volume, the changes are typically propagated to the target volume a few seconds after the data is written to the source volume. However, changes can occur on the source volume before the target volume verifies that it received the change. Because consistent copies of data are formed on the secondary site at set intervals, data loss is determined by the amount of time since the last consistency group was formed. If the system fails, Global Mirror might lose some data that was transmitted when the failure occurred.

For more information, see [Global Replication Services Solution using {{site.data.keyword.IBM_notm}} {{site.data.keyword.powerSys_notm}}](https://cloud.ibm.com/media/docs/downloads/power-iaas/Global_Replication_Services_Solution_using_IBM_Power_Virtual_Server.pdf){: external}. You can also review the [locations](/docs/power-iaas?topic=power-iaas-getting-started-GRS) that support a global replication service.

Consider the {{site.data.keyword.IBM_notm}} toolkit for IBM i from {{site.data.keyword.IBM_notm}} Technology Expert Labs for disaster recovery automation functions and capabilities on the {{site.data.keyword.cloud_notm}} by integrating {{site.data.keyword.powerSys_notm}} with the capabilities of GRS. With the toolkit, simplify and automate the operations of the disaster recovery solution. {{site.data.keyword.IBM_notm}} toolkit for IBM i Full System Replication enables automated disaster recovery functions and capabilities on the {{site.data.keyword.cloud_notm}} by integrating {{site.data.keyword.powerSys_notm}} with the capabilities of GRS. Clients can manage their DR environment that uses their existing IBM i skills. Review the following toolkit functions:

- Full System Replication for {{site.data.keyword.IBM_notm}} IBM i {{site.data.keyword.powerSys_notm}}:
    - Replicate your data from between {{site.data.keyword.cloud_notm}} sites
    - Replicate the IBM i OS volumes

- Disaster recovery services:
    - Perform a virtual machine switch by deactivating a virtual machine from one site and activating its replicated virtual machine on another site.

- Administrative Functions:
    - Reduce outage time by activating your application on another {{site.data.keyword.IBM_notm}} site while performing required maintenance.

- In order to obtain implementation hours for the labor effort for Global replication Service reach out to our Technology Expert Labs team: [technologyservices@ibm.com](mailto:technologyservices@ibm.com)

- Validate [data center pairings](/docs/power-iaas?topic=power-iaas-getting-started-GRS) available for global replication service.

The following image illustrates the use of GRS as the DR solution between two cloud data centers. 

![GRS](/images/grs.svg "GRS Diagram"){: caption="Figure 3: GRS Architecture" caption-side="bottom"}{: external download="grs.svg"}
