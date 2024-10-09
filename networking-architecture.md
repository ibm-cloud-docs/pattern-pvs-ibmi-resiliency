---

copyright:
  years: 2024
lastupdated: "2024-10-09"

subcollection: pattern-pvs-ibmi-resiliency

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for networking
{: #networking-architecture}

| Architecture decision |Requirement | Alternatives | Decision | Rationale |
|-----|-----|-----|-----|------|
| Bring Your Own IP and Edge Gateway | The capability that is needed for customers to provide isolation, security, and edge routing services. | Edge gateways: Palo Alto, Fortinet, and F5 with the client choice. | Gateway: Client choice \n {{site.data.keyword.vpc_short}} facilitates Bring Your Own IP | Edge gateway is a client choice based on the requirements \n \n The client can [bring their own subnet](/docs/vpc?topic=vpc-configuring-address-prefixes) IP address range to an {{site.data.keyword.vpc_full}} \n \n Generic Routing Encapsulation (GRE) Tunnel connecting the {{site.data.keyword.powerSysShort}} to VPC for routes to be advertised across on-premises environment |
| Network segmentation and Isolation | Deploy workloads in an isolated environment and enforce information flow policies. | - Subnets \n - Security groups \n - ACLs \n - Workspaces | VPCs and subnets \n \n Separate {{site.data.keyword.powerSysShort}} LPARs | Native VPC isolation by using separate VPCs and subnets environments for separation of the workload \n \n {{site.data.keyword.powerSysShort}} isolation \n \n Security group with inbound rule, address prefix, and subnet for Secure Automated Backup with Compass. |
| Cloud native connectivity to cloud services | Provide secure connection to cloud services | - VPC Gateway and Virtual Private Endpoints (VPE) \n - Private cloud service endpoints \n - Public cloud service endpoints | VPC Gateway and Virtual Private Endpoints (VPE) | VPC Gateway and Virtual Private Endpoints enable connectivity to {{site.data.keyword.cloud_notm}} services by using private IP addresses allocated from a VPC subnet.|
| Cloud landing zone connectivity | Connect across multiple VPCs and to {{site.data.keyword.cloud_notm}} classic and {{site.data.keyword.powerSysShort}} environments | {{site.data.keyword.tg_short}} (TGW) \n Power Edge Router (PER) \n Global {{site.data.keyword.tg_short}} (GTGW) | {{site.data.keyword.tg_short}} \n \n Power Edge Router (PER) | {{site.data.keyword.tg_short}}s (TGW) are used for interconnectivity between {{site.data.keyword.powerSysShort}} and VPCs. {{site.data.keyword.tg_short}}s have built in redundancy. TGWs are regional and are deployed two per Multi-Zone Region (MZR) within the same region. \n \n Power Edge Routers (PER) are also deployed as two per region. PER is used for interconnectivity between {{site.data.keyword.powerSysShort}} and the TGW. For more information, see [Getting started with PER](/docs/power-iaas?topic=power-iaas-per).|
| Cloud landing zone connectivity across regions | Connect across regions | Global {{site.data.keyword.tg_short}} (GTGW) | Global {{site.data.keyword.tg_short}} (GTGW) | Interconnects classic, VPCs, and {{site.data.keyword.powerSysShort}} resources across regions. \n \n Connect to environments in other regions for resiliency data replication purposes.|
| Domain Name System (DNS) | Ability to resolve DNS names on site | {{site.data.keyword.dns_full_notm}} | {{site.data.keyword.IBM_notm}} continues to forward or relay the DNS to client DNS Servers onsite | This is the default option in the absence of a specific customer requirement to manage DNS \n \n Name resolution for the backup server connections is required. |
{: caption="Architecture decisions for network" caption-side="bottom"}
