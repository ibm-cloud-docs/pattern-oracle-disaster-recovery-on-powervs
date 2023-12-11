---
copyright:
  years: 2023
lastupdated: "2023-11-28"

subcollection: <repo-name>
---
# Reference Architecture 1: Oracle Database Disaster Recovery on IBM PowerVS Cross Region

Figure 1 shows reference architecture for deploying a disaster recovery solution for Oracle Database across IBM PowerVS regions.


![A screenshot of a computer Description automatically generated](image/e4b139fb7a5331e50a4c5af3cd920b36.jpg)

Figure 1 Oracle Disaster Recovery Solution options across two IBM Power Virtual Server environment regions

The architecture in figure 1 is an expansion of the Power Virtual Server Architecture provided in [section 4](#_Solution_Architecture_-). This reference architecture assumes there are additional, non-Oracle Database x86 workloads that will be hosted in the IBM Cloud VPC environment. Below are key components required for an Oracle Database deployment on Power Virtual Server and x86 workload(s) deployed on VPC in two regions.

## Environment deployed in this reference architecture

- VPC environment
  - Edge VPC cluster: this hosts security components, firewall, and other edge services which are essentials for a secure environment
  - Management VPC cluster: this hosts all the management tool stack needed to manage VPC and PowerVS environments
  - Workload VPC cluster: this is where IBM VPC virtual server instance (VSI) workloads are hosted(includes x86 workloads)
  - Transit gateway: to connect VPC and Power Virtual workspace
  - Direct Link: to connect to IBM cloud from customer’s existing Data center and other regional offices
  - VPN connection: for managed services to provide cloud managed operations
  - Load Balancer: if the customer needs a private application load balancer
  - Cloud internet Service: if public global load balancing or DDoS services are needed
  - Virtual Private end points: for connecting to IBM cloud services over the private network such as Activity Tracker, Event Streams, or Cloud Object Storage
  - Monitoring tools: Activity tracker, Log DNA, etc
  - Backup Environment via IBM Storage Protect or Veeam
- Power VS Environment
  - Workload Power VS cluster: actual Oracle Database instance
  - Storage required for LPARS
  - Cloud Object Storage configured for backup

## Oracle Disaster Recovery using Oracle Data Guard

In this section, we will look at how to utilize Oracle Database Enterprise Edition and use Oracle Data Guard feature for disaster recovery. We will set up a database instance in IBM PowerVS primary region & secondary region and configure Data Guard failover for disaster recovery.

Figure 2 illustrates the reference architecture based on Oracle Data Guard.

- Two IBM Power Virtual Server environment regions i.e., for example FRA AZ1 (FRA02) and MAD AZ1 (AZ1)
- Oracle DB is installed on IBM Power Virtual Server LPARS in two Separate Regions
- Oracle DB Enterprise Edition which includes Data Guard, is used to provide real-time replications across the two separate Oracle Database in each region over a Global Transit Gateway
- Primary is Frankfurt and Secondary is Madrid region

![A screenshot of a computer Description automatically generated](image/3f699a0b8bdb16bf395c7c11b98f3636.png)

Figure 2 Oracle Disaster Recovery across different IBM PowerVS region using Oracle Data Guard

## Deployment Architecture Guidance

Here are the key steps for setting up the Oracle database and Data Guard in IBM Power Systems Virtual Server.

- Ensure you have a proper connection established between your on-premises, Data center to IBM Power Systems virtual Server regions.
- [Validate network connectivity](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-network-architecture-diagrams)
- [Configure private subnet](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-configuring-subnet)
- [Configure VPN for Managed service team such as Day 2 operation team who will need VPN access](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-VPN-connections)
- [Site to Site VPN for Resiliency purpose](https://cloud.ibm.com/media/docs/downloads/power-iaas-tutorials/PowerVS_VPN_Tutorial_v1.pdf)
- [Configure and integrate with VPC environment](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-powervs-integration-x86-workloads)
- [Create Primary LPARS which will run Database servers](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-creating-power-virtual-server#creating-power-virtual-server) (both sites: Primary and secondary)
- Create and attach disks for Oracle software provided by IBM PowerVS (tier 1)
- Optional If you want to import boot image check [here](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-importing-boot-image)
- Perform Operating System (OS) prerequisite steps
- [Install Oracle Database according to Oracle guidance and recommendations](https://docs.oracle.com/en/database/oracle/oracle-database/23/dgbkr/oracle-data-guard-broker-installation-requirements.html#GUID-21393DF3-FD7E-44AA-A90C-6533E03CBDDA)
- Install and configure the Oracle Data Guard software

Note: Best practices is to have non Production environment similar to production setup, but non-production can have tier 3 storage and lower CPU as they don’t have high performance requirements.

## Network Architecture Guidance

![A diagram of a server Description automatically generated](image/37a404a1fbce991ea5793bc8ac74fd0f.jpg)

Figure 3 IBM Power Virtual server Environment networking tolopogy

Figure 3 shows required network components from customer Data center to IBM Power Systems Virtual Server

Please ensure you have a proper networking architecture and connection established from your on-premises, Data center to IBM Power Systems virtual server workplace. Please look for guidance on [IBM docs](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-network-architecture-diagrams)

1. Establish Direct 2.0 Link to VPC IBM Cloud account
2. Establish a GRE tunnel (GREb) between the Gateway Router in VPC (NGFW) and the PowerVS Layer 3 device (ensure you have right credentials)
3. Enable Transit Gateway to connect PowerVS and VPC. Have the right VLAN created when you setup your account for VPC and PowerVS
4. PowerVS Connection to LTGW and GTGW
5. Establish a GRE tunnel (GREa) between the Gateway Router in VPC (NGFW) **and the** customer router

## Backup & Restore Architecture Guidance

Database backups are essential for a successful local failure recovery. Backup architecture and backup schedules need to be properly planned to meet RTO/RPO requirements. We will look at general guidance from Power Virtual Server and Oracle Database respectively.

- All Production Databases require Backup/Restore infrastructure
- Oracle RMAN is used for database backup
- Backup Capacity: typically, 2x compression ratio at Storage level is achieved in Oracle DB without performance impact
- See [Backup strategies for IBM Power Systems Virtual Servers](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-backup-strategies) for additional detail
- See resilency section for backup architecture decisions

## Disaster Recovery Architecture Guidance

When designing a disaster recovery (DR) architecture with Oracle Data Guard, it is essential to consider both the local high availability and the remote disaster recovery requirements.

- For transactional systems, like Oracle database, the recommendation to ensure high performance, transactional integrity and recoverability, is asynchronous replication to reduce the latency and bandwidth requirements of data transfer. This allows faster and efficient data replication, without affecting the performance or availability of your primary database
- Network: Ensure a robust and low-latency network connection between the primary and DR sites, preferably using a Global Transit Gateway
- Provide DR governance with regularly scheduled testing (DR Drills)
- Active Data Guard in addition to Data Guard, allows for the database to be open for read-only access, while being kept in sync with the primary database. When using Data Guard for read-only option, the sync process needs to be paused. (Active data guard is an additional DB Option and additional License cost. [https://www.oracle.com/assets/technology-price-list-070617.pdf](https://www.oracle.com/assets/technology-price-list-070617.pdf))
