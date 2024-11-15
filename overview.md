---

copyright:
  years: 2024
lastupdated: "2024-11-08"

subcollection: pattern-pvs-ibmi-resiliency

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Overview
{: #overview}

The deploying resilient IBMi workloads on {{site.data.keyword.powerSys_notm}} pattern deploy a multi-region solution on {{site.data.keyword.powerSysShort}} that includes backup, high availability, and disaster recovery. This pattern can be used to provide an all-in approach for deploying a resilient power architecture within the {{site.data.keyword.powerSysShort}} for IBM i workloads. 

The solution does not address application or database level high available design.
{: note}

- It can support up to 99.99% infrastructure availability when deployed as a multi-region.
- It can support local high availability to protect from immediate LPAR failure.
- It can support backups for IBM i workloads to protect against data loss and can support disaster recovery scenarios.
- It can support out of region disaster recovery.

The {{site.data.keyword.powerSysShort}} resiliency pattern is intended to:

- Accelerate and simplify solution design by providing a standard {{site.data.keyword.cloud_notm}} deployment architecture reference following the {{site.data.keyword.IBM_notm}} Architecture Design Framework.
- Provide a prescriptive, end-to-end enterprise-class solution design with diagrams, component architecture decisions, and rationale for cloud component selection for a resilient architecture.
- Helps ensure that requirements can be met from a performance, system availability, and security perspective.

## Pattern considerations
{: #considerations}

- Backup the IBM i data by using a customer managed backup service.
- Provide OS level local high availability between two IBM i LPARs.
- Provide a disaster recovery solution by using SAN to SAN replication between two regions.

You can validate that offerings are available in the regions you are deploying by using the [{{site.data.keyword.cloud_notm}} portal](https://cloud.ibm.com/login){: external}.
{: note}

Resiliency needs to be considered for both the infrastructure and application levels. This pattern doesn't address application or database level high availability design. Consider latency between environments before you decide on a replication method and options for synchronous and asynchronous replication when designing database replication to meet the requirements.
