---

copyright:
  years: 2024
lastupdated: "2024-10-22"

subcollection: pattern-pvs-ibmi-resiliency

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Service management design
{: #service-managment-design}

Typically, service management tools are integrated with a centralized Service Management Ticketing System to provide a single pane of glass for all operations activities. The list of tools is based on client's needs and requirements and what level of management one wants to perform. The service management architecture decision section provides a set of general guidelines and insightful recommendations.

Monitoring the usage and performance of the backup, high availability, and disaster recovery components is required for the service management aspect for the Resiliency for PowerVS workloads pattern.

## Operations management
{: #operations-management}

Operations management is a key aspect of building resilient applications. The following supports the application availability targets.

- Continuously monitor the application and platform infrastructure to detect failures and degradations.
- Integrate automated monitoring with rich notification tools to automate problem resolution and enable a timely response to incidents.

## Monitoring health
{: #monitoring-health}

Monitoring the health of solution components, cloud services, applications, and operational logs are crucial for maintaining enterprise application availability. Through proper operational monitoring, you can determine whether a failover to an alternative site is necessary, or if operations have normalized following a system disruption.

It’s vital to monitor {{site.data.keyword.cloud_notm}} activity for changes and potential threats that might affect application availability. Implement incident detection, notification, escalation, discovery, and declaration to provide realistic, achievable objectives that add business value.

- Use [{{site.data.keyword.monitoringlong_notm}}](/docs/monitoring?topic=monitoring-about-monitor) and [{{site.data.keyword.logs_full_notm}}](/docs/cloud-logs) to monitor operational metrics and logs. Operational metrics include CPU and memory usage, API response times, and so on.
- Monitor platform metrics from resources in your {{site.data.keyword.powerSysFull}} workspace by using {{site.data.keyword.monitoringlong}} dashboards. To monitor platform metrics for a service instance, provision the {{site.data.keyword.monitoringlong}} instance in the same region where the {{site.data.keyword.cloud_notm}} service instance to be monitored is provisioned.
- Use [{{site.data.keyword.logs_full_notm}}](/docs/cloud-logs) to monitor audit logs for the application, {{site.data.keyword.cloud_notm}} infrastructure, and other {{site.data.keyword.cloud_notm}} services.
- Configure {{site.data.keyword.monitoringlong_notm}}, {{site.data.keyword.logs_full_notm}}, and {{site.data.keyword.cloudaccesstraillong_notm}} to set up alerts, send notifications, and trigger actions in response to the alerts.

- Use [{{site.data.keyword.en_full_notm}}](/docs/event-notifications?topic=event-notifications-en-about) to route events associated with {{site.data.keyword.cloud_notm}} resources (event sources) to a destination such as delivery target for a notification, to trigger actions and help automate the response to critical incidents. Define and build event notifications by linking event sources and destinations. For instance, choose event sources to identify critical events at cloud, network, security, and application levels, and integrate them with targets. Select destinations such as ServiceNow to collect all events and assign owners and AIOps tool to automate response to events like the file system is full.

- The primary role of PowerHA SystemMirror for i in IBM i systems is to provide high availability (HA) and disaster recovery (DR) solutions.

- FalconStor offers several alerting and monitoring features through its StorSight management console. These features include:
  - Real-time data and visibility: Provides status, health, and integrity monitoring of servers and resources.
  - Smart Rules, analytics, reports, and forecasts: Helps in proactive monitoring and issue prevention.
  - Secure multi-tenancy with role-based administration: Ideal for Managed Service Providers (MSPs) offering backup-as-a-service.
  - Single view of replication, deduplication, and retention: Offers a unified view across on-premises and cloud environments for hybrid cloud backup and recovery.
  
  These features help optimize data protection, disaster recovery, high availability, and business continuity operations.

- Alternatively, third-party software such as Splunk and Datadog can be integrated with {{site.data.keyword.cloud_notm}} to provide security monitoring, compliance reporting, and operational intelligence.

## Service management references
{: #sm-reference}

Review the references for service management for {{site.data.keyword.powerSysFull}}:

- [Monitoring metrics for {{site.data.keyword.powerSysFull}}](/docs/power-iaas?topic=power-iaas-monitor-sysdig)
- [Activity tracker](/docs/power-iaas?topic=power-iaas-at-events)

{{site.data.keyword.cloudaccesstraillong}} services are deprecated and will no longer be supported as of 30 March 2025. The replacement service, {{site.data.keyword.logs_full_notm}} is planned to be generally available late second quarter 2024. For more information, see [Getting started with {{site.data.keyword.logs_full_notm}}](/docs/cloud-logs?topic=cloud-logs-getting-started).{: important}
