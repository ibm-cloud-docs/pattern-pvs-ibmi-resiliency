---

copyright:
  years: 2024
lastupdated: "2024-10-22"

subcollection: pattern-pvs-ibmi-resiliency

keywords:

authors:
- name: Jose Ocasio and Anne Mcdermaid

version: 1.0

deployment-url:

docs: /docs/pattern-pvs-aix-resiliency

content-type: reference-architecture

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.powerSysFull}} resiliency on AIX
{: #power-virtual-server-on-AIX}
{: toc-content-type="reference-architecture"}
{: toc-version="1.0"}

This is a baseline solution pattern containing the design and architecture decisions for a PowerVS resiliency solution for AIX workloads to meet common requirements as described in this use case. Actual solutions depend on the specific requirements that are set by the client. Review the following summary of the use case for this reference architecture:

![AIX resiliency summary](/images/usecase.svg "Reference Summary"){: caption="Reference Architecture Summary for Deploying Resilient AIX workloads on {{site.data.keyword.powerSysFull}}" caption-side="bottom"}{: external download="usecase.svg"}

## Architecture diagram
{: #architecture-diagram}

![AIX reference architecture](/images/resiliencypvsarchnumbered.svg "Resiliency Architecture Diagram"){: caption="Deploying resilient AIX workloads on {{site.data.keyword.powerSysFull}} reference architecture" caption-side="bottom"}{: external download="resiliencypvsarchnumbered.svg"}

Review the environments that are related to this reference architecture:

1. Provider connects the environment by using a direct link for private connectivity.
2. The direct link then connects to a Local Transit Gateway. This advertises and routes on-premises traffic to VPC for gateway or firewall inspection.
3. The transit gateway connects to management VPC, which hosts your Next-Generation Firewall, management subnets for your bastion hosts, and your Virtual Private Endpoint.
4. Backup as a Service VPC is deployed as part of the backup service automation not for workloads.
5. The Cobalt Iron VPE instance then communicates to the Cobalt Iron SaaS {{site.data.keyword.Bluemix_notm}} service.
6. PowerVS workspace is deployed within the {{site.data.keyword.powerSysFull}} environment and connects to the Power Edge Router (PER).
7. A local Power high availability standard cluster is then deployed within the workspace to provide local clustering.
8. The management and workload VPC mentioned from the primary site is also deployed in disaster recovery.
9. Global Replication Service (GRS) is deployed as part of Disaster Recovery Storage Area Network to Storage Area Network replication.
10. There is a GRS controller Logical Partition that is deployed at both the primary and the disaster recovery (DR) site.
11. Communication for GRS SAN to SAN traffic between sites occurs over the {{site.data.keyword.IBM_notm}} private backbone.
12. Replication of the controller Logical Partitions occurs over the Global Transit gateway.

## Design scope
{: #design-scope}

The PowerVS resiliency for AIX workloads architecture covers design considerations and architecture decisions for the following aspects and domains:

- Compute: Virtual Servers

- Storage: Primary Storage, Backup Storage, Archive Storage

- Networking: Enterprise Connectivity, BYOIP/Edge gateways, Segmentation, Isolation, Cloud Native Connectivity, Domain name system

- Security: Data security, Identity and access management, Infrastructure and endpoint security

- Resiliency: Backups and Restore, High Availability, Disaster Recovery

The Architecture Framework provides a consistent approach to design cloud solutions by addressing requirements across a set of "aspects" and "domains", which are technology-agnostic architectural areas that need to be considered for any enterprise solution. For more information, see [Introduction to the Architecture Design Framework](/docs/architecture-framework?topic=architecture-framework-intro).

Following the Architecture Design Framework, Resiliency for PowerVS covers design considerations and architecture decisions for the following aspects and domains:

![heatmap](/images/aixheatmap.svg "AIX Heatmap"){: caption="Resiliency for PowerVS AIX Workloads Heat Map" caption-side="bottom"}{: external download="aixheatmap.svg"}

## Requirements
{: #requirements-list}

| Aspect         | Requirements                                                                                                                                                                                                                                                                                  |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Compute            | Provide CPU and RAM to support resiliency components.                                                                                                                                                                                                                                             |
| Storage            | Provide storage to support replication activities. Provide storage to support customer retention schedules.                                                                                                                                                                                       |
| Networking         | Provide enterprise to cloud network connectivity to recovery site. Provide private connectivity between workloads across protected and recovery sites. Deploy workloads in an isolated environment and enforce information flow policies. Provide BYOIP, Edge Routing, VLAN segmentation and DNS |
| Security           | Help ensure data encryption at rest and in transit for the storage layer. Protect the boundaries of the application against denial-of-service and application-layer attacks.                                                                                                                           |
| Resiliency         | Provide local OS level high availability between two AIX LPARs. Provide backups for data retention for AIX workloads. Recovery Time Objective (RTO) and Recovery Point Objective(/RPO) = 1 hours/1 hours.  99.99% Infrastructure Availability                                                     |
| Service Management | Monitor the usage and performance of the resiliency components                                                                                                                                                                                                                                    |
{: caption="Resiliency for PowerVS requirements" caption-side="bottom"}




## Components
{: #component-list}

| Category      | Solution components                                                                                                       | How it is used in a solution                                                                                                      |
|--------------------|-------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| Compute            | PowerVS LPARs                                                                                                                 | - High availability workload virtual servers  \n - Disaster recovery workload virtual servers  \n - Global Replication Service (GRS) controllers |
|                    | VPC VSI                                                                                                                       | Compute for NGFW and management tools                                                                                                  |
| Storage            | Flash Storage from {{site.data.keyword.IBM_notm}} FS9500 series devices                                                                                  | Web, application, database storage, Storage for GRS                                                                                  |
|                    | [Cloud Object Storage](/docs/cloud-object-storage?topic=cloud-object-storage-about-cloud-object-storage) | - Long-term backup archive  \n -  Logs                                                                                                      |
| Networking         | {{site.data.keyword.dl_full_notm}}                                                                                                          | Enterprise to cloud network connectivity                                                                                            |
|                    | Transit Gateway (TGW)                                                                                                         | Connectivity between PowerVS and VPCs                                                                                               |
|                    | Service Endpoints                                                                                                             | Private network access to cloud services such {{site.data.keyword.logs_full_notm}}, Cloud Object Storage.                                                 |
|                    | [Global Transit Gateway (GTGW)](/docs/transit-gateway?topic=transit-gateway-about)                       | Provides PowerVS and VPC connectivity in different regions (global routing)                                                         |
|                    | [DNS Services](/docs/dns-svcs?topic=dns-svcs-about-dns-services)                                         | Private DNS resolution                                                                                                              |
| Security           | Next-Generation Firewall (NGFW)                                                                                               | Provide IDS/IPS and edge firewall capabilities                                                                                      |
| Resiliency         | Secure automated backup with Compass                                                                                          | Backups for AIX workloads                                                                                                           |
|                    | PowerHA Standard                                                                                                              | Local OS level between two LPARS                                                                                                    |
|                    | Global Replication Service and {{site.data.keyword.IBM_notm}} Toolkit for AIX Full System Replication                                                      | SAN to SAN replication between two {{site.data.keyword.cloud_notm}} data centers                                                                           |
| Service Management | {{site.data.keyword.logs_full_notm}} {{site.data.keyword.monitoringlong_notm}}                                                | Apps, Audit, and operational logs monitor platform metrics                                                                          |
{: caption="Resiliency for PowerVS components" caption-side="bottom"}
