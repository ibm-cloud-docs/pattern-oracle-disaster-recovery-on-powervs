---
copyright:
  years: 2023
lastupdated: "2023-12-18"

subcollection: pattern-oracle-disaster-recovery-on-powervs

keywords:
---
{{site.data.keyword.attribute-definition-list}}

# Service management design
{: #design-service-management}

Typically, service management tools are integrated with a centralized Service Management Ticketing System to provide a single pane of glass for all operations activities.

The list of tools are based on clients needs and requirements and what level of management one wants to perform. There are general guidelines and some recommendations in the Service management Architecture decision section.

Operations management is a key aspect of building resilient applications. To support the application availability targets:

* Continuously monitor the application and platform infrastructure to detect failures and degradations.
* Integrate automated monitoring with rich notification tools to automate problem resolution and enable a timely response to incidents.

It is important to monitor the health of all the components of the solution, including infrastructure, cloud services, and application as well as operational logs to detect and correct issues that might affect the availability of enterprise applications. Proper operational monitoring can help you determine whether you need to fail over to an alternative site or whether operations have returned to normal after a system disruption.

Equally important is to track and monitor all activity that is performed on the IBM Cloud to detect changes and potential security threats that might impact the availability of the applications that are deployed on IBM Cloud.

Implement incident detection, notification, escalation, discovery, and declaration to provide realistic, achievable objectives that add business value.

* Use [IBM Cloud Monitoring](/docs/monitoring?topic=monitoring-about-monitor) and[IBM Cloud Logs](/docs/cloud-logs?topic=cloud-logs-getting-started) to monitor operational metrics and logs. Operational metrics include CPU and memory usage, API response times, and so on.
* Use [IBM Cloud Logs](/docs/cloud-logs?topic=cloud-logs-getting-started) to monitor audit logs for the application, IBM Cloud Infrastructure, and other IBM Cloud services.
* Configure IBM Cloud Monitoring and IBM Logs to set up alerts, send notifications, and trigger actions in response to the alerts.
* Use [IBM Cloud Event Notifications](/docs/event-notifications?topic=event-notifications-en-about) to route events associated with IBM Cloud resources (event sources) to a destination (delivery target for a notification) to trigger actions and help automate the response to critical incidents. Define and build event notifications by linking event sources and destinations. As an example, select event sources to detect cloud provider level (for example, region, zone, services), network level (for example load balancers, global load balancers), security level, and application level critical events and integrate them with destination targets. Select destinations such as ServiceNow to collect all events and assign owners and AIOps tool to automate response to events like the file system is full.

Reference for service management for IBM Power Virtual server

* [Monitoring](/docs/power-iaas?topic=power-iaas-monitor-sysdig)
* [Activity Logs](/docs/power-iaas?topic=power-iaas-at-events)
