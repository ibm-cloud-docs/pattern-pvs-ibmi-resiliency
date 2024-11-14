---

copyright:
  years: 2024
lastupdated: "2024-10-23"

subcollection: pattern-pvs-ibmi-resiliency

keywords:

---

# Architecture decisions for security
{: #security-architecture}

The following are security architecture decisions for the Power Virtual Server Resiliency on IBM i pattern.

| Architecture decision | Requirement | Alternatives | Decision | Rationale |
|------|------|-------|-------|-------|
| Encrypt data at rest: Workloads | Ability to encrypt data at rest |  | {{site.data.keyword.powerSys_notm}} storage encryption with provider-managed keys | * {{site.data.keyword.powerSys_notm}} uses {{site.data.keyword.IBM_notm}} FlashSystem Storage with AES-256 (Advanced Encryption Standard) hardware-based encryption \n * For customer-managed keys by selecting a Key Management Service (KMS) for the respective storage service |
| Encrypt data at rest: Backups | Ability to encrypt backups | | Storage Encryption with provider-managed keys | * All objects that are stored in {{site.data.keyword.cos_full_notm}} are encrypted by using randomly generated keys and an all-or-nothing-transform (AONT). \n * FalconStor provides AES-256 encryption for backups through its StorSafe VTL solution, ensuring data security both at rest and during transmission. Additionally, FalconStor offers key management capabilities for greater control over the security of backup archives. |
| Identity Access and Role Management (IDM)   | Securely authenticate users for platform services and control access to resources consistently across {{site.data.keyword.cloud_notm}}| | {{site.data.keyword.cloud_notm}} IAM | * Use IAM access policies to assign users, service IDs, and trusted profiles access to resources within the {{site.data.keyword.cloud_notm}} account.                                                                                                |
| Key Management | Provider-Managed Keys | Key Protect {{site.data.keyword.hscrypto}} | Key Protect | By default, storage at rest is encrypted with provider-managed keys. |
| Privileged Identity and Access Management | Privileged access management services for administrative purposes | BYO Bastion Host, BYO Bastion Host with Privileged Access Management (PAM) Software | BYO Bastion host or Privileged Access Gateway with PAM Software that is deployed in Edge VPC \n 2FA Authentication though {{site.data.keyword.IBM_notm}} Security Verify | Securely access remote resources over the private network for management purposes; bastion accessed through SSH. Session recording, tracking all activities that are successful or not to note any potential threats |
| Core Network Protection | - Strict separation of duties \n * Isolated security zones between environments \n * Isolated, private cloud environment | | Separate VPCs, subnets, Access Control List (ACL), and Security Groups for workloads in VPC. \n \n Use of virtual firewalls that is deployed to the Edge or Transit VPC to provide advance firewall and routing capabilities between VPC and {{site.data.keyword.powerSys_notm}} | * A design combination that uses: \n * Separate VPCs (edge and management) connected through transit gateway and, the use of edge firewall capabilities. \n * Subnets, Security Groups and ACLs to create an Edge or Transit VPC design along with isolated LPARs on {{site.data.keyword.powerSys_notm}} |
| Threat detection and response | * Boundary protection: highest level of isolation from external network threats \n * IPS/IDS protection at all ingress/egress \n * Unified Threat Management (UTM) Firewall | BYO Virtual Firewall \n [FortiGate](https://cloud.ibm.com/catalog/content/ibm-fortigate-AP-HA-terraform-deploy-5dd3e4ba-c94b-43ab-b416-c1c313479cec-global){: external} \n [Palo Alto](https://cloud.ibm.com/catalog/content/ibmcloud-vmseries-1.9-6470816d-562d-4627-86a5-fe3ad4e94b30-global){: external} | BYO Virtual Firewall -  [FortiGate](https://cloud.ibm.com/catalog/content/ibm-fortigate-AP-HA-terraform-deploy-5dd3e4ba-c94b-43ab-b416-c1c313479cec-global){: external} \n [Palo Alto](https://cloud.ibm.com/catalog/content/ibmcloud-vmseries-1.9-6470816d-562d-4627-86a5-fe3ad4e94b30-global){: external} | * Virtual firewall on VSI in the Transit or Edge VPC \n * However, the client preference recommendation is FortiGate \n * FortiGate supports native HA configuration, IPS, and IDS |
{: caption="Security architecture decisions" caption-side="bottom"}
