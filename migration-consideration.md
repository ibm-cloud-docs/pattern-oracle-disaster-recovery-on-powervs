---
copyright:
  years: 2023
lastupdated: "2023-12-18"

subcollection: pattern-oracle-disaster-recovery-on-powervs

keywords:
---
{{site.data.keyword.attribute-definition-list}}

# Migration solution approach
{: #migration-approach}

When workloads are deployed on a new system, you must pay attention to its configuration and tuning to achieve the expected performance. Power Virtual Server uses different IBM Power Systems: E980 (9080-M9S), S922 (9009-22A), and S1022 (9105-22A). For more information, see [Hardware specifications](/docs/power-iaas?topic=power-iaas-about-virtual-server#hardware-specifications).

For AIX, Power Virtual Server supports only AIX 7.1 or later. If you use an unsupported version, it is subject to outages during planned maintenance windows with no advanced notification given. Your current AIX level and POWER processor family can help determine which migration path to follow.

IBM i customers must use IBM i 7.1 or later. Clients running IBM i 6.1 must first upgrade the operating system (OS) to a current support level before you migrate to the Power Systems Virtual Server. IBM i 7.2 supports direct [upgrades from IBM i 6.1 or 7.1 (N-2)](https://www.ibm.com/support/knowledgecenter/ssw_ibm_i_72/rzahc/fastpathrzahc.htm){: external}.

## Migration checklist
{: #migration-checklist}

Before you migrate to a newer IBM Power System, review the following checklist:

1.  Plan the system migration.
1.  Install the latest required software and apply the available fixes.
1.  Set the appropriate processor compatibility mode for logical partitions (LPARs) before and after migration.
1.  Plan the virtual processor (VP) and entitlement for each LPAR to best fit your operation and performance requirements.
1.  Follow the I/O consideration guide.

## Migration approach options
{: #migration approach}

| Approach                                                        | Power VS options                                                                                                                                                                             | Solution guidance                                                                                                                                                                                                                                                                                                                                              |
| --------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| OVA                                                             | Use Power Virtualization Center (PowerVC) to capture an Open Virtualization Appliance (OVA) image of the AIX workload and transfer the OVA to Power Systems Virtual Server                   | * Fully functional PowerVC environment * Sufficient space to capture an OVA image OVA created by PowerVC. Ideal for small environment and noncritical workloads Longer downtime for migration                                                                                                                                                           |
| Use the public Internet                                         | Transfer the system backup by using the public Internet through mksysb backup from an on-premises system to a Power Systems Virtual Server Instance that uses the public network connection. | * Migrations of the backup over the internet requires the following AIX Virtual Server Instance with public Internet interface \n * Access from on-premises network to public Internet AIX mksysb: command is used to generate the backup. \n * The backup can then be migrated over the internet by using the customer's preferred method (for instance sftp) |
| Cloud Object Storage                                            | Transfer the system backup by using Cloud Object Storage and restore it into a Power Systems Virtual Server Instance by using a Linux Virtual Server Instance for staging.                   | You need Aspera to be set up for large volume migration image and data. Secure the connection from a cloud to on-premises environment AIX Virtual Server Instance Direct Link Connect to the IBM Cloud Cloud Object Storage service NFS Server for Linux/AIX mksysb for AIX                                                                                    |
| Veeam                                                           | If the Business Partner or client has licenses, Veeam might be considered.[Veeam Guidance](https://helpcenter.veeam.com/docs/agentforaix/userguide/about.html?ver=40)                           | 1. Set up the Veeam backup and disaster recovery environment on cloud and on-premises \n 2. Install Agents at both sites and configure enterprise connectivity for replication \n 3. Monitor over the migration term \n 4. Switch over to the cloud when the data is transferred over to the cloud.                                                            |
| 3rd Party                                                       | 3rd Party software through Business Partner capability iCluster MIMIX from Syncsort Double-Take® MoveTM for AIX / Maxava HA Zerto or NetBackup                                              | Engage the specific Business Partner. Consider the tool, license cost, software capabilities, and delivery capabilities.                                                                                                                                                                                                                                       |
| GLVM                                                            | Set up GLVM across on-premises and cloud for LPARS asynchronous migration                                                                                                                    | * Look for hardware and LPARS compatibility before use of GLVM (minimum AIX 7.2.5) \n * Set up LPARS across cloud and on-premises \n * Configure GLVM across through agent and configure replication over network after data is fully sync, turn off the source, and switch the primary target to IBM Cloud.                                                   |
| IBM Storage Protect                                             | Use of backup for migration.                                                                                                                                                                 | * Consult your delivery or Business Partner for feasibility \n * Move the backup over to IBM Cloud through Aspera or MDM and restore by using IBM Storage Protect.                                                                                                                                                                                             |
|  |                                                                                                                                                                                              |                                                                                                                                                                                                                                                                                                                                                                |{: caption="Table 1. Migration approach" caption-side="bottom"}


For more information, see [Migration strategies for IBM Power Systems Virtual Servers](/docs/power-iaas?topic=power-iaas-migration-strategies-power).
