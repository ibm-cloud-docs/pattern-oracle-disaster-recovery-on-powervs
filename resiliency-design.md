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

Consider the backup frequency needed to meet the client Recovery Point Objective (RPO). A full backup captures the entire database, while an incremental backup captures only the changes since the last backup. Weekly full backups and daily incremental backups are recommended for Oracle DB. These requirements are strictly determined by the customer based on their business needs.

For Oracle Databases, RMAN tool is recommended for backup.

The table below is an example for a typical backup regime that one can perform, it may vary based on each customer’s requirement and is tied to the RTO/RPO and application backup and restore requirement. The table below is just an example.

Please look at your requirement before you design backup regime.

| **Production** | **Oracle Database** |           |           |
| -------------------- | ------------------------- | --------- | --------- |
|                      | Backup                    | Frequency | Retention |
|                      | Full                      | Weekly    | 1 Week    |
|                      | Incremental               | Daily     | 30 Days   |

For OS level, Storage Protect is recommended for backup.

| **Production** | **OS & File Systems** |           |           |
| -------------------- | --------------------------- | --------- | --------- |
|                      | Backup                      | Frequency | Retention |
|                      | Full                        | Monthly   | 60 Days   |
|                      | Incremental                 | Daily     | 60 Days   |
