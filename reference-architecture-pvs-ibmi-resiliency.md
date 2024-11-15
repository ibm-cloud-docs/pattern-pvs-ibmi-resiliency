---

copyright:
  years: 2024
lastupdated: "2024-11-15"

subcollection: pattern-pvs-ibmi-resiliency

keywords:

authors:
- name: Jose Ocasio and Anne Mcdermaid

version: 1.0

deployment-url:

docs: /docs/pattern-pvs-ibmi-resiliency

content-type: reference-architecture

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.powerSysFull}} resiliency on IBM i
{: #power-virtual-server-on-ibmi}
{: toc-content-type="reference-architecture"}
{: toc-version="1.0"}

This is a baseline solution pattern containing the design and architecture decisions for a PowerVS resiliency solution for IBM i workloads to meet common requirements as described in this use case. Actual solutions depend on the specific requirements that are set by the client. Review the following summary of the use case for this reference architecture:

![IBM i resiliency summary](/images/usecase.svg "Reference Summary"){: caption="Reference Architecture Summary for Deploying Resilient IBM i workloads on {{site.data.keyword.powerSysFull}}" caption-side="bottom"}{: external download="usecase.svg"}

## Architecture diagram
{: #architecture-diagram}

![IBM i reference architecture](/images/resiliencypvsarchnumbered.svg "Resiliency Architecture Diagram"){: caption="Deploying resilient IBM i workloads on {{site.data.keyword.powerSysFull}} reference architecture" caption-side="bottom"}{: external download="resiliencypvsarchnumbered.svg"}

Review the environments that are related to this reference architecture:

1. Provider connects the environment by using a direct link for private connectivity.
2. The direct link then connects to a Local Transit Gateway. This advertises and routes on-premises traffic to VPC for gateway or firewall inspection.
3. The transit gateway connects to management VPC, which hosts your Next-Generation Firewall, management subnets for your bastion hosts, and your Virtual Private Endpoint.
4. Falconstor Storsight VSI is deployed as a monitoring dashboard for the backup solution.
5. A Virtual Private Endpoint is deployed to communicate from the falconstor appliance to the Cloud Services Layer: Cloud Object storage. 
6. PowerVS workspace is deployed within the {{site.data.keyword.powerSysFull}} environment and connects to the Power Edge Router (PER).
7. A local Power high availability standard cluster is then deployed within the workspace to provide local clustering. It is utilizing PowerHA SystemMirror for i as the software and geographic mirroring as the replication method. 
8. The management and workload VPC mentioned from the primary site is also deployed in disaster recovery.
9. Global Replication Service (GRS) is deployed as part of Disaster Recovery Storage Area Network to Storage Area Network replication.
10. There is a GRS controller Logical Partition that is deployed at both the primary and the disaster recovery (DR) site.
11. Communication for GRS SAN to SAN traffic between sites occurs over the {{site.data.keyword.IBM_notm}} private backbone.
12. Replication of the controller Logical Partitions occurs over the Global Transit gateway.

## Design scope
{: #design-scope}

The PowerVS resiliency for IBM i workloads architecture covers design considerations and architecture decisions for the following aspects and domains:

- Compute: Virtual Servers

- Storage: Primary Storage, Backup Storage, Archive Storage

- Networking: Enterprise Connectivity, BYOIP/Edge gateways, Segmentation, Isolation, Cloud Native Connectivity, Domain name system

- Security: Data security, Identity and access management, Infrastructure and endpoint security

- Resiliency: Backups and Restore, High Availability, Disaster Recovery

The Architecture Framework provides a consistent approach to design cloud solutions by addressing requirements across a set of "aspects" and "domains", which are technology-agnostic architectural areas that need to be considered for any enterprise solution. For more information, see [Introduction to the Architecture Design Framework](/docs/architecture-framework?topic=architecture-framework-intro).

Following the Architecture Design Framework, Resiliency for PowerVS covers design considerations and architecture decisions for the following aspects and domains:

![heatmap](/images/ibmiheatmap.svg "IBM i Heatmap"){: caption="Resiliency for PowerVS IBM i Workloads Heat Map" caption-side="bottom"}{: external download="ibmiheatmap.svg"}

## Requirements
{: #requirements-list}

| Aspect         | Requirements                                                                                                                                                                                                                                                                                  |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Compute            | Provide CPU and RAM to support resiliency components.                                                                                                                                                                                                                                             |
| Storage            | Provide storage to support replication activities. Provide storage to support customer retention schedules.                                                                                                                                                                                       |
| Networking         | Provide enterprise to cloud network connectivity to recovery site. Provide private connectivity between workloads across protected and recovery sites. Deploy workloads in an isolated environment and enforce information flow policies. Provide BYOIP, Edge Routing, VLAN segmentation and DNS |
| Security           | Help ensure data encryption at rest and in transit for the storage layer. Protect the boundaries of the application against denial-of-service and application-layer attacks.                                                                                                                           |
| Resiliency         | Provide local OS level high availability between two IBM i LPARs. Provide backups for data retention for IBM i workloads. Recovery Time Objective (RTO) and Recovery Point Objective(/RPO) = 1 hours/1 hours.  99.99% Infrastructure Availability                                                     |
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
| Resiliency         | FalconStor StorSafe VTL                                                                                        | Backups for IBM i workloads                                                                                                           |
|                    | PowerHA SystemMirror for i                                                                                                              | Local OS level between two LPARS                                                                                                    |
|                    | Global Replication Service and {{site.data.keyword.IBM_notm}} Toolkit for IBM i Full System Replication                                                      | SAN to SAN replication between two {{site.data.keyword.cloud_notm}} data centers                                                                           |
| Service Management | {{site.data.keyword.logs_full_notm}} {{site.data.keyword.monitoringlong_notm}}                                                | Apps, Audit, and operational logs monitor platform metrics                                                                          |
{: caption="Resiliency for PowerVS components" caption-side="bottom"}
