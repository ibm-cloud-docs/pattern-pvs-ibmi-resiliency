---

copyright:
  years: 2024
lastupdated: "2024-10-23"

subcollection: pattern-pvs-ibmi-resiliency

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Security design
{: #security-design}

The following are requirements for the security aspect for the Power Virtual server Resiliency on AIX pattern:

- Help ensure data encryption at rest and in transit for the storage layer.

- Protect the boundaries of the application against denial-of-service and application-layer attacks.

## Security design considerations
{: #security-design-IBMi}

Security should be applied at all layers of the solution for defense in depth depending on requirements, for example at the network edge, VPCs, and at every compute instance as well as at the application and database layers. In addition, data should be protected both in transit and at rest according to data classification. Controls should be in place to eliminate the need for direct access to the environment, and when direct access is needed, all access should be logged and monitored.

It is a best practice to isolate, secure, manage, and monitor all ingress and egress traffic to the environment and to centralize these functions in a hub and spoke model through an Edge VPC. As a result, it's recommended that security and isolation are established by using a combination of Security Groups (SG), Network Access Control Lists (NACL) and routing/firewall rules defined in the Edge VPC. This is in addition to the cloud account-level protection provided in {{site.data.keyword.cloud_notm}} Identity Services.

Consider the following:

- Implementation of firewalls in the Transit/Edge can secure and route traffic to the {{site.data.keyword.cloud_notm}} environment as well as from the customer network.

- Enable logging to facilitate the firewall activity analysis if needed to meet client security IPS/IDS requirements.

- All traffic, both public and private, should route through the Edge/Transit VPC for routing, isolation, and logging.

- To restrict, log and monitor administrative access to the environment, a bastion host is recommended in the Edge VPC for all administrative access. In addition, all bastion access and activity should be logged and monitored through Privileged Access Management (PAM) services.
