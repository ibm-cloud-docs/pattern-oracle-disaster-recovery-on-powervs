---
copyright:
  years: 2023
lastupdated: "2023-11-28"

subcollection: <repo-name>

keywords:
---

# Backup and restore design considerations

{: \#backup and restore-architecture}

There is a seamless integration with Oracle Recovery Manager (RMAN) to direct database backup and restore activity to IBM Cloud Object Storage. More information can be found [here](https://www.ibm.com/downloads/cas/O0BZVBPN).

Recovery Time Objective (RTO) and Recovery Point Objective (RPO) are critical design considerations when designing a backup environment. This section is a typical example of what a customer can do using backup tools on IBM environment.

Recovery Manager (RMAN) is the native Oracle Solution that performs backup and recovery tasks for local and clustered databases and automates administration of configured backup strategies. RMAN includes Oracle’s Secure Backup (OSB) cloud module with an SBT (Secure Backup) interface that enables the use of the S3 protocol for data backup to IBM Cloud Object Storage.

Database backups are essential for a successful local failure recovery. Backup architecture and backup schedules need to be properly planned to meet RTO/RPO requirements. We will look at general guidance from Power Virtual Server and Oracle Database respectively.

-   All Production Databases require Backup/Restore infrastructure
-   Oracle RMAN is used for database backup
-   Backup Capacity: typically, 2x compression ratio at Storage level is achieved in Oracle DB without performance impact
-   See [Backup strategies for IBM Power Systems Virtual Servers](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-backup-strategies) for additional detail

Consider the backup frequency needed to meet the client Recovery Point Objective (RPO). A full backup captures the entire database, while an incremental backup captures only the changes since the last backup. Weekly full backups and daily incremental backups are recommended for Oracle DB. These requirements are strictly determined by the customer based on their business needs.

For Oracle Databases, RMAN tool is recommended for backup.

The table below is an example for a typical backup regime that one can perform, it may vary based on each customer’s requirement and is tied to the RTO/RPO and application backup and restore requirement. The table below is just an example.

Please look at your requirement before you design backup regime.

| **Production** | **Oracle Database** |           |           |
|----------------|---------------------|-----------|-----------|
|                | Backup              | Frequency | Retention |
|                | Full                | Weekly    | 1 Week    |
|                | Incremental         | Daily     | 30 Days   |

For OS level, Storage Protect is recommended for backup.

| **Production** | **OS & File Systems** |           |           |
|----------------|-----------------------|-----------|-----------|
|                | Backup                | Frequency | Retention |
|                | Full                  | Monthly   | 60 Days   |
|                | Incremental           | Daily     | 60 Days   |

## Oracle Data Guard Considerations

Oracle Data Guard ensures data protection, and disaster recovery for Oracle Databases. It provides a comprehensive set of services that create, maintain, manage, and monitor one or more standby databases to enable production Oracle databases to survive disasters and data corruptions. By maintaining these standby databases as copies of the production database, if the production database becomes unavailable because of a planned or an unplanned outage, Data Guard can switch standby database to the production role, minimizing the downtime associated with the outage. It can be used with traditional backup, restoration, and cluster techniques to provide a high level of data protection and data availability.

## Deciding Between Full Site Failover or Seamless Connection Failover

-   The first step is to evaluate which failover option best meets your business and application requirements when your primary database or primary site is inaccessible or lost due to a disaster.
-   The following table describes various conditions for each outage type and recommends a failover option in each scenario.
-   The table below provides recommended Failover Options for Different Outage Scenarios

| **Outage Type**                                          | **Condition**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | **Recommended Failover Option**                                                                                                                                                                   |
|----------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Primary Site Failure (including all application servers) | Primary site contains all existing application servers (or mid-tier servers) that were connected to the failed primary database.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Full site failover is required.                                                                                                                                                                   |
| Complete Primary Database or Primary Server Failure      | Application servers are not impacted, and users can reconnect to new primary database in a secondary disaster recovery site. Application performance and throughput is still acceptable with different network latency between application servers and new primary database in a secondary disaster recovery site. Typically, analytical or reporting applications can tolerate higher network latency between client and database without any noticeable performance impact, while Online Transactional Processing (OLTP) applications performance may suffer more significantly if there is an increase in network latency between the application server and database. | If performance is acceptable, seamless connection failover is recommended to minimize downtime and enable automatic application and database failover. Otherwise, full site failover is required. |

Full Site Failover Best Practices

-   A **full site failover** means that the complete site fails over to another site with a new set of application tiers and a new primary database.
-   Complete site failure results in both the application and database tiers becoming unavailable. To maintain availability, application users must be redirected to a secondary site that hosts a redundant application tier and a synchronized copy of the production database.

Note:

In using Active Data Guard, the users doesn’t have to stop the sync process, the user can just read while it is being updated.

## Disaster Recovery Architecture Guidance

When designing a disaster recovery (DR) architecture with Oracle Data Guard, it is essential to consider both the local high availability and the remote disaster recovery requirements.

-   For transactional systems, like Oracle database, the recommendation to ensure high performance, transactional integrity and recoverability, is asynchronous replication to reduce the latency and bandwidth requirements of data transfer. This allows faster and efficient data replication, without affecting the performance or availability of your primary database
-   Network: Ensure a robust and low-latency network connection between the primary and DR sites, preferably using a Global Transit Gateway
-   Provide DR governance with regularly scheduled testing (DR Drills)
-   Active Data Guard in addition to Data Guard, allows for the database to be open for read-only access, while being kept in sync with the primary database. When using Data Guard for read-only option, the sync process needs to be paused. (Active data guard is an additional DB Option and additional License cost. <https://www.oracle.com/assets/technology-price-list-070617.pdf>)
