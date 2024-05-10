---
copyright:
  years: 2023
lastupdated: "2023-12-18"

subcollection: pattern-oracle-disaster-recovery-on-powervs

keywords:

---
{{site.data.keyword.attribute-definition-list}}

# Oracle Data Guard considerations
{: #oracle-considerations}

Oracle Data Guard ensures data protection and disaster recovery for Oracle Databases. It provides a comprehensive set of services that create, maintain, manage, and monitor one or more standby databases to enable production Oracle databases to survive disasters and data corruptions. By maintaining these standby databases as copies of the production database, if the production database becomes unavailable because of a planned or an unplanned outage, Data Guard can switch the standby database to the production role, minimizing the downtime associated with the outage. It can be used with traditional backup, restoration, and cluster techniques to provide a high level of data protection and data availability.

## Deciding between full site failover or seamless connection failover
{: #deciding}

- The first step is to evaluate which failover option best meets your business and application requirements when your primary database or primary site is inaccessible or lost due to a disaster.
- The following table describes various conditions for each outage type and recommends a failover option in each scenario.
- The following table provides recommended Failover Options for Different Outage Scenarios

| Outage type                                    | Condition                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Recommended failover option                                                                                                                                                             |
| -------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Primary site failure (including all application servers) | Primary site contains all existing application servers (or mid-tier servers) that were connected to the failed primary database.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Full site failover is required.                                                                                                                                                                   |
| Complete primary database or primary server failure      | Application servers are not impacted, and users can reconnect to a new primary database in a secondary disaster recovery site. Application performance and throughput are still acceptable with different network latency between application servers and a new primary database in a secondary disaster recovery site. Typically, analytical or reporting applications can tolerate higher network latency between client and database without any noticeable performance impact, while Online Transactional Processing (OLTP) applications performance might suffer more significantly if an increase in network latency between the application server and database occurs. | If performance is acceptable, a seamless connection failover is recommended to minimize downtime and enable automatic application and database failover. Otherwise, a full site failover is required. |
{: caption="Table 1. Various outage conditions" caption-side="bottom"}

## Full site failover best practices
{: #full-site-failover}

- A **full site failover** means that the complete site fails over to another site with a new set of application tiers and a new primary database.
- Complete site failure results in both the application and database tiers becoming unavailable. To maintain availability, application users must be redirected to a secondary site that hosts a redundant application tier and a synchronized copy of the production database.

In using Active Data Guard, the user doesn’t need to stop the sync process. The user can read only while it is being updated.
{: note}
