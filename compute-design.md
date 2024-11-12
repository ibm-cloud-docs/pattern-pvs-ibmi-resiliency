---

copyright:
  years: 2024
lastupdated: "2024-11-12"

subcollection: pattern-pvs-ibmi-resiliency

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Compute design
{: #compute-design}

{{site.data.keyword.powerSysFull}} provides infrastructure by using the latest virtual servers in the form of Logical Partitions (LPARS). An LPAR is a way of subdividing a computer’s resources, including memory, storage, and processors, into small logical units. These LPARs are available with various vCPU and RAM combinations, allowing users to define configurations that align with their specific use case requirements. 

The requirements for the resiliency for {{site.data.keyword.powerSysFull}} IBM i workloads pattern focus on the following:

- The compute aspects required for the backup components.
- The compute aspects required for the disaster recovery workloads.
- The compute aspects required to support high availability activities.

## Compute considerations for backups
{: #design-considerations-backups}

Review the following backup method for a FalconStor StorSafe Virtual Tape Library: 

FalconStor StorSafe Virtual Tape Library (VTL) is a software solution that optimizes backup and restore to improve performance and significantly reduce backup storage costs, all without requiring changes to the existing requirement. With its integrated deduplication, the solution removes redundant copies of data, reducing capacity requirements, decreasing storage costs, and minimizing replication and restore times. StorSafe VTL can be used with all leading backup solutions, and enables both hybrid and native-cloud backup, as well as both workload and tape migration to the cloud. StorSafe VTL also supports NFS and SMB interfaces in a NAS environment. Falconstor can be configured and ordered by using the [Falconstore tile](https://cloud.ibm.com/catalog/content/vtltile-tags-v10.03-01-f1e88e51-7e3d-4fbc-a7ed-3ab9adb2afea-global?catalog_query=aHR0cHM6Ly9jbG91ZC5pYm0uY29tL2NhdGFsb2c%2Fc2VhcmNoPWZhbGNvbnN0b3Ijc2VhcmNoX3Jlc3VsdHM%3D){: external} in the cloud catalog. 

For the Falconstor appliance setup, it's recommended to deploy two appliances: one in the production environment and another at the disaster recovery site. These two devices replicate with each other to prevent outages or data corruption at either location. If only a single Falconstor VTL is deployed, there is no disaster recovery protection for that appliance, which means a potential risk of losing all backups.

## Compute considerations for high availability
{: #design-considerations-ha}

{{site.data.keyword.IBM_notm}} PowerHA SystemMirror for i, formerly known as {{site.data.keyword.IBM_notm}} PowerHA and HACMP, is an {{site.data.keyword.IBM_notm}} solution for high-availability clusters on IBM i, Unix, and Linux platforms. It provides near-continuous application availability with advanced failure detection, failover, and recovery capabilities. 

The {{site.data.keyword.powerSys_notm}} Resiliency on IBM i Pattern involves several compute considerations that must be addressed for high availability. These considerations are essential for helping ensure a robust and reliable system.

- {{site.data.keyword.IBM_notm}} PowerHA SystemMirror Standard Edition allows the failover of 2 LPAR sharing the storage volume.

- If one LPAR fails, the second LPAR would seamlessly assume the primary role, helping ensure uninterrupted system functions. 

- This method offers adequate LPAR compute for clustered IBM i LPARs in one {{site.data.keyword.cloud_notm}} data center for local high availability.

## Compute considerations for disaster recovery
{: #design-considerations-dr}

{{site.data.keyword.IBM_notm}} Global Replication Service (GRS) is a powerful solution that provides asynchronous data replication for {{site.data.keyword.IBM_notm}}i workloads. GRS helps ensure data resilience by replicating storage volumes from one {{site.data.keyword.IBM_notm}} data center to a geographically distant {{site.data.keyword.IBM_notm}} data center. It supports failover and failback mechanisms, allowing seamless transitions between primary and secondary sites.

These are key considerations for a successful disaster recovery setup of Power Virtual Server workloads in the {{site.data.keyword.powerSys_notm}} resiliency pattern.

- For this pattern, the chosen method involves deploying a secondary site by using Global Replication Service (GRS) as the replication mechanism. 

- The requirements for the GRS control LPARS are at least one control LPAR (.25 cpu x 16 GB x300GB) per data center and per OS type. The control LPAR is responsible for managing GRS operations. It coordinates data sharing and replication across systems within the same SYSPLEX, a set of interconnected systems. The control LPAR helps ensure consistency, failover, and failback mechanisms during disaster recovery scenarios. It handles serialization, buffering, and metadata management for GRS-enabled datasets.

- Use LPARS for Disaster Recovery Workloads at the secondary data center 

- Help ensure sufficient LPAR provisioning in the recovery location to support replicas of critical workloads if a disaster occurs.

- Consider workload optimizations for secondary site LPARS such as shared processor pools.

- The shared processor pool (SPP) allocates processor capacity among multiple virtual server instances. Unlike dedicated instances, SPP reserves cores for guaranteed availability, especially in high availability and disaster recovery scenarios.

- Minimum shared processor virtual machines can be deployed upfront to maintain replication workloads to grow and shrink the capacity of the virtual machines when needed.

- The shared processor pool allows the provisioning of the disaster recovery environment at the lowest possible compute configuration. At the time of the disaster recovery event, the LPARs are rehydrated from the reserved compute capacity.

- Optimize the shared processor pool costs by paying for compute capacity when needed.

The shared processor pool (SPP) reserves only compute capacity, not the memory. For more information, see [The shared processor pool](/docs/power-iaas?topic=power-iaas-manage-SPP) and in [Managing shared processor pools](https://www.ibm.com/docs/en/power9?topic=systems-managing-shared-processor-pools){: external}. For more information on the Global Replication Service, see [GRS](/docs/power-iaas?topic=power-iaas-getting-started-GRS)
