---

copyright:
  years: 2024
lastupdated: "2024-11-12"

subcollection: pattern-pvs-ibmi-resiliency

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Network design
{: #network-design}

Network design is a pivotal element in crafting a resilient architecture. It outlines the interconnections and communication pathways among various solution components. This process entails choosing optimal network connections for replication, while also considering the effects of bandwidth and distance on network latency. In this section, the essential networking considerations for enhancing resilience through effective network design are discussed.

This resiliency pattern uses a two-region deployment for disaster recovery. The requirements for the network aspect for the resiliency on {{site.data.keyword.powerSysShort}} workloads pattern focus on:

- Resilience for enterprise connectivity to the {{site.data.keyword.powerSysShort}} disaster recovery environment
- Connectivity between data centers

## Network design considerations
{: #network-design-consideratons}

### Network latency
{: #network-latency}

Network latency refers to the amount of time that it takes for data to travel from one point to another across a network. In our pattern, latency effects two aspects: high availability and disaster recovery. 

Review the latency considerations based on this pattern:

- This pattern is using Global Replication Services (GRS) which operates between two sites that are over 300 km apart. 
- Greater distances typically need asynchronous replication. This is because asynchronous replication is designed to work over longer distances. 
- Depending on the application, synchronous mirroring might be the only practical approach.

### Replication traffic
{: #replication-traffic}

Replication traffic refers to the data transmitted between systems during the process of replicating or copying information from one location to another. 

Review the key considerations for replication traffic when implementing backup and disaster recovery:

- Global replication traffic between {{site.data.keyword.powerSysShort}} regions traverses the {{site.data.keyword.cloud_notm}} backbone.
- GRS control Logical Partition (LPAR) traffic traverses the GTGW.
- Backup replication flows as follows: From Falconstor Site 1 to the Power Edge Router (PER), then through the Transit Gateway (TGW), followed by the Global Transit Gateway (GTGW). It continues to Site 2 TGW, then to Site 2 PER, and finally reaches the second Falconstor appliance.

## Virtual Private Cloud (VPC)
{: #virtual-private-cloud}

Multiple VPCs are used in this pattern. Additional client requirements might require additional VPCs. This pattern includes:

- Management VPC: Next Generation Firewall (NGFW) is deployed in the Management VPC. To provide isolation and centralized advanced security functions, the network design follows the hub and spoke VPC model. The management VPC serves as the hub for which all ingress and egress traffic flows. The management VPC is a virtual network VPC that acts as a central point of connectivity to on-premises network and all other VPCs. {{site.data.keyword.powerSysShort}} workspaces are connected to the management VPC also know as the hub by a {{site.data.keyword.tg_short}}, which allows traffic routing between the VPCs and {{site.data.keyword.powerSysShort}} workspaces in the {{site.data.keyword.cloud_notm}} account.
- Workload VPC: The FalconStor management console is deployed within the workload VPC, which functions as the spoke VPC. This environment supports ancillary workload infrastructure, including virtual servers and bare metal instances.

### Falconstor Virtual Tape Library (VTL)
{: #falconstor-vtl}

When provisioned through the {{site.data.keyword.cloud_notm}} catalog, an automation process deploys the FalconStor VTL solution, which includes:

- The deployment target on Power Virtual Server
- A delivery method of a server image. This may be an S922, E980, or an E1022 logical partition (LPAR) Power Virtual server model. 
- The provisioning of the Falconstor license.
- There are optional configurable input variables available to the customer such as network, storage sizing, and index size. 
- For more information, see the FalconStor [order form](https://cloud.ibm.com/catalog/content/vtltile-tags-v10.03-01-f1e88e51-7e3d-4fbc-a7ed-3ab9adb2afea-global){: external}.

### High availability clusters
{: #highavailability-clusters}

Each {{site.data.keyword.powerSysShort}} needs its own IP which is a service IP typically on the same VLAN as the partner Power VSI. The service IP is an extra IP used for the application that is impacted by HA.
