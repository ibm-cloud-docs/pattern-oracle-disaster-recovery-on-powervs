---
copyright:
  years: 2023
lastupdated: "2023-11-28"

subcollection: <repo-name>

keywords:
---
Backup Architecture Decisions

Power Systems Virtual Server users can implement any compatible agent-based backup for AIX virtual machines (VM). *Veeam for AIX* and *IBM Storage Protect* are two commonly used backup strategies.

Note: IBM Storage Protect is recommended. As an alternative option, Veeam is supported as well.

*See* [*backup strategies*](https://www.ibm.com/docs/fr/power-systems-vs?topic=strategies-backup-power-systems-virtual-servers#backup-aix) *for more information and* [*implementation guides*](https://cloud.ibm.com/media/docs/downloads/power-iaas-tutorials/PowerVS_AIX_Backups_Tutorial_v1.pdf)*.*

| Aspects              | **Domains**  | **Requirements** | **Chosen Service**         | **Decisions / Rationale**                                                                                                                                                                             |
| -------------------- | ------------------ | ---------------------- | -------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Resiliency** | Backup and Restore |                        |                                  |                                                                                                                                                                                                             |
| **Resiliency** | Backup - PowerVS   | Backup LPARS           | Image Capture and Snapshot       | Additional ICOS is required Suitable for system disks only backup/restore                                                                                                                                   |
| **Resiliency** | Backup - PowerVS   | Backup LPARS           | IBM Storage Protect (SP) for AIX | Deployment Options: SP on AIX LPAR in PowerVS SP on IBM Cloud VPC Instance                                                                                                                                  |
| **Resiliency** | Backup - PowerVS   | Backup LPARS           | Veeam Backup Restore for AIX     | Network Connection Required to Storage Target endpoint Veeam Agent for IBM AIX can be configured and installed,[please look for Veeam AIX guidance.](https://www.veeam.com/ibm-aix-oracle-solaris-backup.html) |
| **Resiliency** | Backup – Oracle   | Backup Database        | RMAN                             | RMAN is a native backup and recovery solution. For further best practices from oracle, click[here](https://www.oracle.com/docs/tech/oda-backup-recovery-technical-brief.pdf)                                   |
