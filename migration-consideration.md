---
copyright:
  years: 2023
lastupdated: "2023-11-28"

subcollection: <repo-name>

keywords:

---

{{site.data.keyword.attribute-definition-list}}

{: \#Migration solution design consideration }

# Migration Solution Approach

| Approach                | Power VS options                                                                                                                                                                  | Solution Guidance                                                                                                                                                                                                                                                                                                                              |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **OVA**                 | Leverage Power Virtualization Center (PowerVC), to capture an Open Virtualization Appliance (OVA) image of the AIX workload and transfer the OVA to Power Systems Virtual Server  | Fully functional PowerVC environment Sufficient space to capture an OVA image OVA created by PowerVC  Ideal for small environment and noncritical workloads Longer downtime for migration                                                                                                                                                      |
| **Use public Internet** | Transfer System Backup Using the Public Internet via mksysb backup from an on-premises system to a Power Systems Virtual Server Instance using the public network connection.     | Migrations the backup over the internet and requires the following AIX Virtual Server Instance with Public Internet interface Access from On Premise network to Public Internet AIX mksysb: command is used to generate the backup. Thebackup can then be migrated over the internet using the customer's preferred method (for instance sftp) |
| **COS**                 | Transfer System Backup Using Cloud Object Storage and restore it into a Power Systems Virtual Server Instance using a Linux Virtual Server Instance for staging.                  | You will need Aspera to be setup for large volume migration image and data). Secure the connection from cloud to on-premises environment AIX Virtual Server Instance Direct Link Connect to IBM Cloud Cloud Object Storage Service NFS Server for Linux/AIX mksysb for AIX                                                                     |
| **Veeam**               | Veeam may be considered if the business partner or client has licenses.  [Veeam Guidance](https://helpcenter.veeam.com/docs/agentforaix/userguide/about.html?ver=40)              | Setup Veeam backup and Disaster recovery environment on cloud and On-premises Install Agents at both sites and configure enterprise connectivity for replication Monitor over the migration term. Switch over to cloud when data is transferred over to cloud.                                                                                 |
| **3rd Party**           | 3rd Party software via business partner capability iCluster MIMIX from Syncsort Double-Take® MoveTM for AIX / Maxava HA Zerto or **NetBackup**                                    | Engage the specific Business Partner. Consider the tool, license cost, software capabilities, and delivery capabilities.                                                                                                                                                                                                                       |
| **GLVM**                | **Setup GLVM across On-premises and cloud for LPARS asynchronous migration**                                                                                                      | Look for hardware and LPARS compatibility before use of GLVM (minimum AIX 7.2.5) Setup LPARS across cloud and on-premises Configure GLVM across via agent and configure replication over network Once data is fully syn, turn of the source and switch primary target to IBM cloud.                                                            |
| **IBM Storage protect** | **Use of backup for migration.**                                                                                                                                                  | Consult your delivery or business partner for feasibility Move the backup over to IBM cloud via Aspera or MDM and restore using IBM Storage protect.                                                                                                                                                                                           |

<https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-migration-strategies-power>
