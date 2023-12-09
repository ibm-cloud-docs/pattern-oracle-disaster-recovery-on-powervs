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

## Backup Architecture Decisions

Power Systems Virtual Server users can implement any compatible agent-based backup for AIX virtual machines (VM). *Veeam for AIX* and *IBM Storage Protect* are two commonly used backup strategies.

Note: IBM Storage Protect is recommended. As an alternative option, Veeam is supported as well.

*See* [*backup strategies*](https://www.ibm.com/docs/fr/power-systems-vs?topic=strategies-backup-power-systems-virtual-servers#backup-aix) *for more information and* [*implementation guides*](https://cloud.ibm.com/media/docs/downloads/power-iaas-tutorials/PowerVS_AIX_Backups_Tutorial_v1.pdf)*.*

| \*\*Aspects \*\* | **Domains**        | **Requirements** | **Chosen Service**               | **Decisions / Rationale**                                                                                                                                                                                      |
|------------------|--------------------|------------------|----------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Resiliency**   | Backup and Restore |                  |                                  |                                                                                                                                                                                                                |
| **Resiliency**   | Backup - PowerVS   | Backup LPARS     | Image Capture and Snapshot       | Additional ICOS is required Suitable for system disks only backup/restore                                                                                                                                      |
| **Resiliency**   | Backup - PowerVS   | Backup LPARS     | IBM Storage Protect (SP) for AIX | Deployment Options: SP on AIX LPAR in PowerVS SP on IBM Cloud VPC Instance                                                                                                                                     |
| **Resiliency**   | Backup - PowerVS   | Backup LPARS     | Veeam Backup Restore for AIX     | Network Connection Required to Storage Target endpoint Veeam Agent for IBM AIX can be configured and installed,[please look for Veeam AIX guidance.](https://www.veeam.com/ibm-aix-oracle-solaris-backup.html) |
| **Resiliency**   | Backup – Oracle    | Backup Database  | RMAN                             | RMAN is a native backup and recovery solution. For further best practices from oracle, click[here](https://www.oracle.com/docs/tech/oda-backup-recovery-technical-brief.pdf)                                   |
