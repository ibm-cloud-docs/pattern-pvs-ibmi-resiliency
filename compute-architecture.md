---

copyright:
  years: 2026
lastupdated: "2026-03-30"

subcollection: pattern-pvs-ibmi-resiliency

keywords: compute, architecture compute

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for compute
{: #compute-decisions}

| Architecture decision | Requirement | Decision | Rationale |
|---------------------------------------------|---------------------------------------------------------------|------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| High Availability (HA) workloads within the same AZ  | Provide compute for Logical Partitions (LPARS) supporting high availability within the same AZ  | Power Virtual Server LPARs deployed within the same AZ | To achieve HA within the same AZ, use PowerHA SystemMirror. Ensure that the resources can handle workloads during failover scenarios. In this case shared volumes can be used between the nodes, placed in an IASP on both cluster hosts. This is good for customers who do not want to have extra costs paying for Enhanced PowerHA with GeoMirror. |
| High Availability (HA) workloads between two AZs| Provide compute for Logical Partitions (LPARS) supporting high availability in each AZ| Power Virtual Server LPARs deployed in two different AZs | To achieve HA between two different AZs, use PowerHA systemMirror Enhanced Edition and GeoMirror. As we do not have shared volumes in between to AZs, GeoMirror will keep the data synchronized in either sync or async mode. This solution is for customers who require greater "resource distance" between their HA nodes for extra costs. |
| Disaster Recovery with Global Replication Service (GRS) | Provide compute for Logical Partitions in both regions | Power Virtual Server LPARs deployed in two different regions with GRS enabled volumes | The compute resources are used for the protected LPARs in region 1 and their secondary LPARs in region 2. This solution does not offer any automation, hence it has reduced costs and requires more manual tasks |
| Disaster Recovery with Global Replication Service (GRS) using Disaster Recovery Automation (DRA) | Provide compute for Logical Partitions in both regions and provide compute for the ksys orchestrator LPAR or LPARs. | Power Virtual Server LPARs deployed in two different regions with ksys orchestrator LPARs in HA mode, each ksys LPAR is placed into a different region | The compute resources are used for the protected LPARs in region 1 and their secondary LPARs in region 2 along with the ksys orchestrator LPARs. This solution will automate the failover of protected LPARs, but has a higher cost. |
| Edge and management VPCs | Provide compute for workloads in the edge and management VPCs | Virtual Servers for VPC | Deploy virtual servers to handle workloads in both the edge and management VPCs. |
| Disaster recovery workloads        | Provide compute for LPARs supporting disaster recovery. | Power Virtual Server LPARs | Target an environment to match specific workload requirements. \n \n For your disaster recovery environment, consider leveraging shared processor pools to manage CPU resources efficiently. By adopting this approach, you can flexibly allocate compute capacity while maintaining compliance and minimizing costs.  |
{: caption="Architecture decisions for compute" caption-side="bottom"}
