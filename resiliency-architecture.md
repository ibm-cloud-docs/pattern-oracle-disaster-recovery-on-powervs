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

IBM Storage Protect is recommended. As an alternative option, Veeam is supported as well. {: note}

For more information, see [backup strategies](https://www.ibm.com/docs/fr/power-systems-vs?topic=strategies-backup-power-systems-virtual-servers#backup-aix){: external} and [implementation guides](https://cloud.ibm.com/media/docs/downloads/power-iaas-tutorials/PowerVS_AIX_Backups_Tutorial_v1.pdf){: external}.

| **Architecture decision** | **Requirements** | **Decision**                     | **Rationale**                                                                                                                                                                                                          |
|---------------------------|------------------|----------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Backup - PowerVS          | Backup LPARS     | Image Capture and Snapshot       | Additional ICOS is required Suitable for system disks only backup/restore                                                                                                                                              |
| Backup - PowerVS          | Backup LPARS     | IBM Storage Protect (SP) for AIX | Deployment Options: SP on AIX LPAR in PowerVS SP on IBM Cloud VPC Instance                                                                                                                                             |
| Backup - PowerVS          | Backup LPARS     | Veeam Backup Restore for AIX     | Network Connection Required to Storage Target endpoint Veeam Agent for IBM AIX can be configured and installed, [please look for Veeam AIX guidance.](https://www.veeam.com/ibm-aix-oracle-solaris-backup.html)        |
| Backup – Oracle           | Backup Database  | RMAN                             | RMAN is a native backup and recovery solution. For further best practices from oracle, please follow the guidance of RMAN at [Oracle portal](https://www.oracle.com/docs/tech/oda-backup-recovery-technical-brief.pdf) |

{: caption="Architecture decisions for backup" caption-side="bottom"}
