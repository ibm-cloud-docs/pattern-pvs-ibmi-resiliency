---

copyright:
  years: 2024
lastupdated: "2024-11-04"

subcollection: pattern-pvs-ibmi-resiliency

keywords: compute, architecture compute

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for compute
{: #compute-decisions}

| Architecture decision | Requirement | Decision | Rationale |
|---------------------------------------------|---------------------------------------------------------------|------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Local High Availability (HA) workloads  | Provide compute for Logical Partitions (LPARS) supporting local high availability  | Power Virtual Server LPARs | To achieve local HA, use PowerHA Geographic Mirror. Make sure that the resources can handle workloads during failover scenarios. |
| Global Replication Service (GRS) controllers | Provide compute for replication components. | Power Virtual Server LPARs | The compute resources are used for GRS controller workloads. |
| Edge and management VPCs | Provide compute for workloads in the edge and management VPCs | Virtual Servers for VPC | Deploy virtual servers to handle workloads in both the edge and management VPCs. |
| Disaster recovery workloads        | Provide compute for LPARs supporting disaster recovery. | Power Virtual Server LPARs | Target an environment to match specific workload requirements. \n \n For your disaster recovery environment, consider using shared processor pools to manage CPU resources efficiently. By adopting this approach, you can flexibly allocate compute capacity while maintaining compliance and minimizing costs.  |
{: caption="Architecture decisions for compute" caption-side="bottom"}
