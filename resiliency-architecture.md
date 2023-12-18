---
copyright:
  years: 2023
lastupdated: "2023-12-18"

subcollection: pattern-oracle-disaster-recovery-on-powervs

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for backup
{: #architecture-backup}

Power Systems Virtual Server users can implement any compatible agent-based backup for AIX virtual machines (VM). Veeam for AIX and IBM Storage Protect are two commonly used backup strategies.

IBM Storage Protect is recommended. As an alternative option, Veeam is supported as well.
{: note}

For more information, see [backup strategies](https://www.ibm.com/docs/fr/power-systems-vs?topic=strategies-backup-power-systems-virtual-servers#backup-aix){: external} and [implementation guides](https://cloud.ibm.com/media/docs/downloads/power-iaas-tutorials/PowerVS_AIX_Backups_Tutorial_v1.pdf){: external}.

| Aspects              | Domains  | Requirements | Chosen service         | Decisions or Rationale                                                                                                                                                                             |
| -------------------- | ------------------ | ---------------------- | -------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Resiliency | Backup and restore |                        |                                  |                                                                                                                                                                                                             |
| Resiliency | Backup - PowerVS   | Backup LPARS           | Image Capture and Snapshot       | * Additional IBM Cloud Object Storage is required \n * Suitable for system disk back up/restore only                                                                                                                                   |
| Resiliency | Backup - PowerVS   | Backup LPARS           | IBM Storage Protect (SP) for AIX | Deployment Options: SP on AIX LPAR in PowerVS SP on IBM Cloud VPC Instance                                                                                                                                  |
| Resiliency | Backup - PowerVS   | Backup LPARS           | Veeam Backup Restore for AIX     | Network Connection Required to Storage Target endpoint Veeam Agent for IBM AIX can be configured and installed. For more information, see [Veeam AIX guidance](https://www.veeam.com/ibm-aix-oracle-solaris-backup.html){: external} |
| Resiliency | Backup – Oracle   | Backup database        | RMAN                             | RMAN is a native backup and recovery solution. For more information, see [best practices from oracle](https://www.oracle.com/docs/tech/oda-backup-recovery-technical-brief.pdf){: external}                                   |
{: caption="Table 1. Architecture decisions for backup" caption-side="bottom"}
