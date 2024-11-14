---

copyright:
  years: 2024
lastupdated: "2024-10-23"

subcollection: pattern-pvs-ibmi-resiliency

keywords:

---

# Architecture decisions for service management
{: #service}

The following sections summarize the architecture decisions for service management for the web app multi-zone resiliency pattern.


| Architecture decision | Requirement | Alternatives | Decision | Rationale |
|------|------|-------|------|------|
| Operational Monitoring of cloud infrastructure and services | Monitor system health to detect issues that might impact the availability of the system and determine the need to failover to an alternative site. | {{site.data.keyword.monitoringlong_notm}}  BYO tool | {{site.data.keyword.monitoringlong_notm}} | * {{site.data.keyword.monitoringlong_notm}} provides visibility, alerting, and troubleshooting for PowerVS workspaces. \n * {{site.data.keyword.monitoringlong_notm}} collects and monitors operational metrics for cloud infrastructure as well as the cloud platform and services and provides a single view for all metrics \n * Monitoring of PowerVC metrics with {{site.data.keyword.monitoringlong_notm}} dashboards that are operated by Sysdig in partnership with {{site.data.keyword.IBM_notm}} /n * Cloud Native tools such as NMON can also be used. |
| Logging of cloud infrastructure and services | Log monitoring of cloud infrastructure and services | [{{site.data.keyword.logs_full_notm}}](/docs/cloud-logs?topic=cloud-logs-getting-started) \n BYO Tool | [{{site.data.keyword.logs_full_notm}}](/docs/cloud-logs?topic=cloud-logs-getting-started) | * Recommended tool for infrastructure logging for any non-VMWare workloads. \n Ingestion and integration with other tools for diagnosis and alerts \n * Ingestion and integration with other tools for diagnosis and alerts \n * {{site.data.keyword.logs_full_notm}} collects operational logs from applications, platform resources, and infrastructure and provides interfaces to view and analyze all logs. |
| Alerting                                                    | Provide a mechanism to identify and send notifications about operational issues that are found across application and infrastructure | {{site.data.keyword.monitoringlong_notm}} + {{site.data.keyword.cloud_notm}} Logging + Event Notifications \n BYO Tool | {{site.data.keyword.monitoringlong_notm}} + {{site.data.keyword.cloud_notm}} Logging + Event Notifications | * {{site.data.keyword.monitoringlong_notm}} and {{site.data.keyword.cloud_notm}} logging support the configuration of alerts to detect operational issues and send notifications to targeted channels. \n * Event Notifications are used to route the alert events to service destinations to automate response actions. \n * Full stack observability for application and infrastructure when combined with Pager Duty + ServiceNow (SNOW) + Customer SIEM                                                     |
{: caption="Service management architecture decisions" caption-side="bottom"}
