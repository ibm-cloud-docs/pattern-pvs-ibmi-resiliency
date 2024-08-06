---

copyright:
  years: 2024
lastupdated: "2024-08-06"

subcollection: pattern-pvs-ibmi-resiliency

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Network design
{: #network-design}

Network design is a pivotal element in crafting a resilient architecture. It outlines the interconnections and communication pathways among various solution components. This process entails choosing optimal network connections for replication, while also taking into account the effects of bandwidth and distance on network latency. In this section, we delve into the essential networking considerations for enhancing resilience through effective network design.

This resiliency pattern uses a two-region deployment for disaster recovery. The requirements for the network aspect for the resiliency on {{site.data.keyword.powerSysShort}} workloads pattern focus on:

- Resilience for enterprise connectivity to the {{site.data.keyword.powerSysShort}} disaster recovery environment.
- Connectivity between data centers

## Network Design Considerations
{: #network-design-consideratons}

### Network latency
{: #network-latency}

Network latency refers to the amount of time it takes for data to travel from one point to another across a network. In our pattern latency effects 2 aspects. The first one is high availibiility and the second is disaster recovery. 

Here are some latency considerations based on our pattern:

- This Pattern is using Global Replication Services (GRS) which operates between two sites that are over 300 km apart. 
- Greater distances typically need asynchronous replication. This is because asynchronous replication is designed to work over longer distances. 
- Depending on the application, synchronous mirroring might be the only practical approach.

### Replication Traffic
{: #replication-traffic}

Replication traffic refers to the data transmitted between systems during the process of replicating or copying information from one location to another. 

Here are some key considerations for replication traffic when implementing backup and disaster recovery:

- Global replication traffic between {{site.data.keyword.powerSysShort}} regions traverse the {{site.data.keyword.cloud_notm}} backbone.
- GRS control Logical Partition (LPAR) traffic traverses the GTGW.
- Backup replication flows in the following manner: TGW -> VPE -> {{site.data.keyword.cloud_notm}} Backbone-Compass Vault System -> {{site.data.keyword.cloud_notm}} Backbone -> Secondary Compass vault system

## Virtual Private Cloud (VPC)
{: #virtual-private-cloud}

Multiple VPCs are used in this pattern. Additional client requirements might require additional VPCs. This pattern includes:

- BaaS and backup VPC: Secure Automated Backup with Compass automated deployment deploys a BaaS and backup VPC. 

    We recommend refraining from deploying additional workloads in this VPC, as it is intended exclusively for the backup solution.
    {: note}

- Edge VPC: Next Generation Firewall (NGFW) is deployed in the Edge VPC. To provide isolation and centralized advanced security functions, the network design follows the hub and spoke VPC model. The Edge VPC serves as the hub for which all ingress and egress traffic flows. The Edge is a virtual network VPC that acts as a central point of connectivity to on-premises network and all other VPCs. {{site.data.keyword.powerSysShort}} workspaces are connected to the Edge also know as the Hub by a {{site.data.keyword.tg_short}}, which allows traffic routing between the VPCs and {{site.data.keyword.powerSysShort}} workspaces in the {{site.data.keyword.cloud_notm}} account.

### Secure automated backup with Compass
{: #secure-automated-backup-compass}

When provisioned through the {{site.data.keyword.cloud_notm}} catalog, an automation process deploys the backup solution, which includes:

- {{site.data.keyword.vpc_full}} (VPC) exclusive use of the backup activity (“Baas/Backup vpc”)
- Virtual Private Endpoints (VPE) to establish a secure private network connection to the Compass backup servers.
- A local {{site.data.keyword.tg_short}} if it does not exist. The cloud account will notify you if one does exist. 
- The backup offering VPC and the {{site.data.keyword.powerSysFull}} workspaces should be in the same region and connected by using the local {{site.data.keyword.tg_short}}.

### High availability clusters
{: #highavailability-clusters}

Each {{site.data.keyword.powerSysShort}} needs its own IP, “service IP” typically on the same VLAN as the partner Power VSI. The service IP is an "additional" IP used for the application being HA'd.
