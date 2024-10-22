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

The following are requirements for the service management aspect for the Resiliency for PowerVS workloads pattern:

- Monitor the usage and performance of the backup, high availability, and disaster recovery components.

Operations management is a key aspect of building resilient applications. The following supports the application availability targets.

- Continuously monitor the application and platform infrastructure to detect failures and degradations.

- Integrate automated monitoring with rich notification tools to automate problem resolution and enable a timely response to incidents.

Monitoring the health of solution components, cloud services, applications, and operational logs are crucial for maintaining enterprise application availability. Through proper operational monitoring, you can determine whether a failover to an alternative site is necessary, or if operations have normalized following a system disruption.

It’s vital to monitor {{site.data.keyword.cloud_notm}} activity for changes and potential threats that might affect application availability.

Implement incident detection, notification, escalation, discovery, and declaration to provide realistic, achievable objectives that add business value.

- Use [{{site.data.keyword.monitoringlong_notm}}](/docs/monitoring?topic=monitoring-about-monitor) and [{{site.data.keyword.la_full}}](https://cloud.ibm.com/docs/log-analysis?topic=log-analysis-getting-started) to monitor operational metrics and logs. Operational metrics include CPU and memory usage, API response times, and so on.

- Monitor platform metrics from resources in your IBM® Power® Virtual Server workspace by using {{site.data.keyword.monitoringlong}} dashboards. To monitor platform metrics for a service instance, provision the {{site.data.keyword.monitoringlong}} instance in the same region where the {{site.data.keyword.cloud_notm}} service instance to be monitored is provisioned.

- Use [{{site.data.keyword.cloudaccesstraillong}}](/docs/activity-tracker?topic=activity-tracker-getting-started) to monitor audit logs for the application, {{site.data.keyword.cloud_notm}} Infrastructure, and other {{site.data.keyword.cloud_notm}} services.

- Configure {{site.data.keyword.monitoringlong_notm}}, {{site.data.keyword.logs_full_notm}}, and {{site.data.keyword.cloudaccesstraillong_notm}} to set up alerts, send notifications, and trigger actions in response to the alerts.

- Use [{{site.data.keyword.en_full_notm}}](/docs/event-notifications?topic=event-notifications-en-about) to route events associated with {{site.data.keyword.cloud_notm}} resources (event sources) to a destination (delivery target for a notification) to trigger actions and help automate the response to critical incidents. Define and build event notifications by linking event sources and destinations. For instance, choose event sources to identify critical events at cloud, network, security, and application levels, and integrate them with targets. Select destinations such as ServiceNow to collect all events and assign owners and AIOps tool to automate response to events like the file system is full.

- The primary task of PowerHA® SystemMirror® is to recognize and respond to failures. PowerHA SystemMirror uses the Cluster Aware AIX® infrastructure to monitor the activity of its network interfaces, devices, and IP labels. Monitoring connections are necessary because they enable PowerHA SystemMirror to recognize the difference between a network failure and a node failure. If the PowerHA SystemMirror network (whose IP labels are used in a resource group) loses connectivity, it detects the cluster network failure and takes steps to avoid cluster partitioning. PowerHA SystemMirror automatically monitors interfaces on TCP/IP networks, Storage Area Networks, and Repository disks.

- Secure automated backup with Compass - Alerting, notifications, and ticketing features and integration with {{site.data.keyword.cloud_notm}} Monitoring, {{site.data.keyword.logs_full_notm}}, and Client tools.

- Alternatively, third-party software such as Splunk and Datadog can be integrated with {{site.data.keyword.cloud_notm}} to provide security monitoring, compliance reporting, and operational intelligence.

Here are some references for service management for {{site.data.keyword.powerSysFull}}.

- [Monitoring metrics for {{site.data.keyword.powerSysFull}}](/docs/power-iaas?topic=power-iaas-monitor-sysdig)

- [Activity tracker](/docs/power-iaas?topic=power-iaas-at-events)

{{site.data.keyword.cloudaccesstraillong}} services are deprecated and will no longer be supported as of 30 March 2025. The replacement service, {{site.data.keyword.logs_full_notm}} is planned to be generally available late second quarter 2024. For more information, see [Getting started with {{site.data.keyword.logs_full_notm}}](/docs/cloud-logs?topic=cloud-logs-getting-started).{: important}
