---

copyright:
  years: 2024
lastupdated: "2024-08-06"

subcollection: pattern-pvs-ibmi-resiliency

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Compute design
{: #compute-design}

{{site.data.keyword.powerSysFull}} provides infrastructure by using the latest Virtual Servers in the form of Logical Partitions (LPARS). An LPAR (Logical Partition) is a way of subdividing a computer’s resources, including memory, storage, and processors, into small logical units. These LPARs are available with various vCPU and RAM combinations, allowing users to define configurations that align with their specific use case requirements. 

The requirements for the resiliency for {{site.data.keyword.powerSysFull}} AIX workloads pattern focus on the following:

-  The compute aspects required for the backup components.
-  The compute aspects required for the disaster recovery workloads.
-  The compute aspects required to support high availability activities.

## Compute considerations for backups
{: #design-considerations-backups}

Review the following backup method for a Secure Automated Backup with Compass: 

This service is a fully managed backup as a service solution for any AIX workloads. It is ordered and configured by using the [Managing shared processor pools](https://cloud.ibm.com/catalog/services/secure-automated-backup-with-compass){: external}. Compute is deployed as a service and connected to the {{site.data.keyword.powerSysFull}} workloads through a Backup as a Service VPC and a transit gateway in the client {{site.data.keyword.Bluemix_notm}} account.

Compass backup servers are preconfigured in data centers and are also replicated across to the other regions. When a customer provisions the backup offering through IBM Cloud catalog, an automated process deploys the necessary components, including Virtual Private Cloud (VPC) and Virtual Private Endpoints (VPE), to establish a secure private network connection to the Compass backup servers.

It is advisable to avoid allocating any additional resources to the backup VPC, as this VPC is exclusively dedicated to the compass solution.
{: tip}

## Compute considerations for high availability
{: #design-considerations-ha}

{{site.data.keyword.IBM_notm}} PowerHA SystemMirror, formerly known as {{site.data.keyword.IBM_notm}} PowerHA and HACMP, is an {{site.data.keyword.IBM_notm}} solution for high-availability clusters on AIX Unix and Linux platforms. It provides near-continuous application availability with advanced failure detection, failover, and recovery capabilities. 

The {{site.data.keyword.powerSys_notm}} Resiliency on AIX Pattern involves several compute considerations that must be addressed for high availability1. These considerations are essential for ensuring a robust and reliable system.

- {{site.data.keyword.IBM_notm}} PowerHA SystemMirror Standard Edition allows the failover of 2 LPAR sharing the storage volume

- In the event that one LPAR fails, the second LPAR would seamlessly assume the primary role, ensuring uninterrupted system functionality. 

- This method offers adequate LPAR compute for clustered AIX LPARs in one {{site.data.keyword.cloud_notm}} data center for local high availability.

## Compute considerations for disaster recovery
{: #design-considerations-dr}

{{site.data.keyword.IBM_notm}} Global Replication Service (GRS) is a powerful solution that provides asynchronous data replication for {{site.data.keyword.IBM_notm}} i, AIX, and Linux workloads. GRS ensures data resilience by replicating storage volumes from one {{site.data.keyword.IBM_notm}} data center to a geographically distant {{site.data.keyword.IBM_notm}} data center. It supports failover and failback mechanisms, allowing seamless transitions between primary and secondary sites.

These are key considerations for a successful disaster recovery setup of Power workloads in the {{site.data.keyword.powerSys_notm}} Resiliency Pattern.

- For this pattern, the chosen method involves deploying a secondary site using Global Replication Service (GRS) as the replication mechanism. 

- The requirements for the GRS control LPARS are at least onne control LPAR (Logical Partition) (.25 cpu x 16 GB x300GB) per data center and per OS type. The control LPAR is responsible for managing GRS operations. It coordinates data sharing and replication across systems within the same SYSPLEX (a set of interconnected systems). The control LPAR ensures consistency, failover, and failback mechanisms during disaster recovery scenarios. It handles serialization, buffering, and metadata management for GRS-enabled datasets.

- LPARS for Disaster Recovery Workloads at the secondary data center. 

- Ensure sufficient LPAR provisioning in the recovery location to support replicas of critical workloads in the event of a disaster.

- Consider workload optimizations for secondary site LPARS such as shared processor pools.

- The shared processor pool (SPP) allocates processor capacity among multiple virtual server instances. Unlike dedicated instances, SPP reserves cores for guaranteed availability, especially in high availability and disaster recovery scenarios.

- Minimum shared processor virtual machines can be deployed upfront to maintain replication workloads to grow and shrink the capacity of the virtual machines when needed.

- The shared processor pool allows the provisioning of the disaster recovery environment at the lowest possible compute configuration. At the time of the disaster recovery event, the LPARs are rehydrated from the reserved compute capacity.

- Optimize the shared processor pool costs by paying for compute capacity when needed.

The shared processor pool (SPP) reserves only compute capacity, not the memory. For more information, see [The shared processor pool](/docs/power-iaas?topic=power-iaas-manage-SPP) and in [Managing shared processor pools](https://www.ibm.com/docs/en/power9?topic=systems-managing-shared-processor-pools){: external}.

For more information on the Global Replication Service, see [GRS](/docs/power-iaas?topic=power-iaas-getting-started-GRS)
