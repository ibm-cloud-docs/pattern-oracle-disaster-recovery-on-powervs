---
copyright:
  years: 2024
lastupdated: "2024-09-10"

keywords: # Not typically populated

subcollection: pattern-oracle-disaster-recovery-on-powervs
authors:
  - name: Dwarkanath P Rao
    url: https://www.linkedin.com/in/dwarakanath/

# The release that the reference architecture describes
version: 1.0

# Use if the reference architecture has deployable code.
# Value is the URL to land the user in the IBM Cloud catalog details page for the deployable architecture.
# See https://test.cloud.ibm.com/docs/get-coding?topic=get-coding-deploy-button
deployment-url:

docs: https://cloud.ibm.com/docs/pattern-oracle-disaster-recovery-on-powervs
# use-case from 'code' column in
# https://github.ibm.com/digital/taxonomy/blob/main/topics/topics_flat_list.csv
use-case: cloud

content-type: reference-architecture
---
{{site.data.keyword.attribute-definition-list}}

# Oracle Database Disaster Recovery between Customer DC and IBM PowerVS

{: #Oracle-dr-ibm-clientdc-pvs}
{: toc-content-type="reference-architecture"}
{: toc-use-case="cloud"}
{: toc-version="1.0"}

## Architecture diagram
{: #architecture-diagram}

This reference architecture covers a solution overview and details on how to design an Oracle Disaster recovery architecture on IBM Power Virtual Server environment and customer's existing data center.

We assume that the primary workload is hosted on IBM Power Systems Virtual Server workspace and the Disaster recovery is hosted in a customer or 3rd Party Data center, however the workloads can also be reversed where the secondary workload is in IBM PowerVS and the primary workload is in the customer Data center.

### Architecture overview
{: #architecture-overview}

The following figure shows high-level deployment architecture with Oracle Data Guard and without Oracle Data Guard. It considers IBM PowerVS hosting as the primary site and the customer Data Center as the secondary site (DR). IBM VPC hosts common shared services such as IAM, DNS, Monitoring, management tools, and x86 workloads.

![Figure IBM Power Virtual Server environment and on-premises overview diagram featuring VPC and PowerVS workspace](image/pvs-on-ibm-pvs-ibm-cloud-to-on-prem.svg){: caption="IBM Power Virtual Server environment and on-premises overview" caption-side="bottom"}

- Primary environment is the IBM Frankfurt region and the secondary environment is the customer’s existing data center.
- Configure network connectivity that is accomplished through Direct Links to the primary region with VPN access for Managed Service Providers (MSPs).
- An Edge VPC is deployed which contains routing and security functions. For security purposes, all ingress and egress traffic routes through the Edge VPC. It contains an SFTP server, Bastion host (jump), Firewalls providing advanced security functions.
- The Edge VPC is connected to the PowerVS environment through a local Transit Gateway. The PowerVS environment hosts the Oracle Database, the rest of the customer environment is hosted in VPC.
- Public connectivity routes through Cloud Internet services that can provide load balancing, failover, and DDoS services, then routes to the edge VPC
- The VPC connections to the PowerVS environment through a TGW and GRE tunnel.
- Virtual Private endpoints are used to provide connectivity to cloud native services from each VPC.
- Ensure that the connectivity from customer environment is established to IBM PowerVS and security aspects are considered. To address the networking requirements, ensure to follow the steps at the following [link](/docs/power-iaas?topic=power-iaas-network-architecture-diagrams).
- Oracle Database is installed in LPARS with multiple Tier 1 LUNs for Operating System and Database that are used for boot and database executable and to store Database schema.

### Oracle disaster recovery that uses Oracle Data Guard
{: #oracle-data-guard}

The Oracle Disaster recovery solution approach across IBM PowerVS and customer data center uses Oracle Data Guard to achieve Disaster recovery of Oracle Database across IBM Power Virtual Server and the customer Data Center.

![Figure Oracle Disaster Recovery across Power Virtual Server environment and on-premises using Oracle DG replication methods](image/pvsibm-onprem-Oracle-across-IBM&on-Premises.svg){: caption="Oracle Disaster Recovery across Power Virtual Server environment and on-premises" caption-side="bottom"}

The reference architect to host Oracle Database and x86 workloads on IBM cloud, includes key components that are required for oracle deployment on the Power Virtual Server environment.

### Environment that is deployed as a part of this reference architecture
{: #environment-deployed}

The primary environment workloads that are hosted in IBM include:

- VPC environment

  - Edge VPC cluster: hosts security components, firewall, and other edge services that are essential for your environment.
  - Management VPC cluster: hosts all the management tool stacks needed to manage VPC and Power VS environments.
  - Workload VPC cluster: hosts IBM VPC Virtual Server Instance (VSI) workloads. (x86 workloads).
  - Transit gateway: connects VPC and Power Virtual workspace.
  - Direct Link: connectivity to IBM cloud from customer’s existing Data center and other regional offices.
  - VPN connection: for managed services to provide cloud-managed operations.
  - Load Balancer: if the customer external load balancing.
  - Cloud internet Service: if DDoS and global load balancing is required.
  - Virtual Private end points: are used to provide connectivity to cloud native services from each VPC. Monitoring tools: IBM Cloud Logs, IBM Cloud Monitoring, and customer’s custom tools.
  - Backup Environment through IBM Storage Protect or Veeam.
- Power VS Environment

  - Workload power VS cluster: actual Oracle Database primary instance.
  - Storage required for LPARS.
  - Cloud Object Storage configured for backup.
- Customer Data Center: This is the disaster recovery site.

  - Ensure that you have all hardware components that are configured to meet your primary workload requirements by running in the IBM Power Virtual Server environment.
  - Ensure compute, storage, networking, security requirements, and prerequisites are addressed for Oracle to replicate across both the sites.

Carefully consider options for synchronous and asynchronous replication when you are designing database replication to meet the client requirements. It is recommended that you discuss latency between environments before you decide on a replication method for Oracle.
{: note}

### Software components used for the Oracle Data Guard deployment
{: #software-components}

Table showing the components used for Oracle replication

| Software component                                               | Target system component | Description                                               |
| ---------------------------------------------------------------- | ----------------------- | --------------------------------------------------------- |
| Database replication Data Guard                                  | Installed on LPAR       | Used for Oracle Database replication across another site. |
| {: caption="Software components" caption-side="bottom"} |                         |                                                           |

Active Data Guard enables you to read while the data is syncing. There is no need to pause they sync process.
{: note}

### Deployment architecture guidance

{: #deployment-guidance}

Ensure that you complete the prerequisites for IBM Power Virtual Server environment connectivity. Follow the necessary steps for a secured connection needed for a Power Virtual server environment. The Customer Data center connection to IBM Power virtual servers details are described in the [Cloud Docs](/docs/power-iaas?topic=power-iaas-network-architecture-diagrams). For more information, see:

- [Connecting to VPC through Direct link](/docs/power-iaas?topic=power-iaas-ordering-direct-link-connect).
- [Connecting to Customer DC through Mega port](/docs/power-iaas?topic=power-iaas-network-architecture-diagrams#network-reference-architecture-onprem).

To configure the Power Virtual Server and Oracle Database:

1. [Install and Configure AIX LPARs and LUNs](/docs/power-iaas?topic=power-iaas-creating-power-virtual-server).
2. Install Oracle Database  and configure LUNs on tier 1.
3. Configure and test Oracle Data Guard across both the sites to ensure that replication across sites meets your objective.

## Design scope
{: #design-scope}

This document provides design recommendations for an Oracle Database deployment on IBM Power Virtual Server environment to meet disaster recovery requirements. It covers resiliency patterns:

* Disaster recovery patterns
* IBM Power Virtual Server environment and Customer Data center (hybrid cloud scenario)
* Disaster recovery of Oracle Database that uses Oracle Data Guard

Following the [Architecture Design Framework](/docs/architecture-framework?topic=architecture-framework-intro), the Resiliency Patterns cover design considerations for the following aspects and domains:

The Oracle disaster recovery on IBM Power Virtual Systems Server architecture covers design considerations and architecture decisions for the following aspects and domains (as defined in the [Architecture Design Framework](/docs/architecture-framework?topic=architecture-framework-intro)):

- **Compute:** Virtual Servers
- **Storage:** Primary Storage, Backup Storage
- **Networking:** Enterprise Connectivity, Segmentation and Isolation, Cloud Native Connectivity, Load Balancing, Domain Name System
- **Security:** Data Security, Identity and Access Management, Application Security, Infrastructure and Endpoint Security
- **Resiliency:** High Availability, Backup and Restore,
- **Service Management:** Monitoring, Logging, Auditing, Alerting

![Oracle Data Guard architecture design scope](image/pvs-on-ibm-heat-map.drawio.svg){: caption="Architecture Design Framework" caption-side="bottom"}

The Architecture Design Framework provides a consistent approach to design cloud solutions by addressing requirements across a set of "aspects" and "domains", which are technology-agnostic architectural areas that need to be considered for any enterprise solution. See [Introduction to the Architecture Design Framework](/docs/architecture-framework?topic=architecture-framework-intro) for more details.

## Requirements
{: #requirements}

The following represents a baseline set of requirements that we believe are applicable to most clients and critical to successful Oracle Disaster Recovery deployment. This set of requirements are key considerations for a successful Disaster recovery setup of Power workloads and other co-existing applications in IBM Power Systems Virtual Server environments, IBM Cloud, and customer Data Centers.

| Aspect                                                    | Requirements                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| --------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Compute                                                   | Provide properly isolated compute resources with adequate compute capacity for the applications.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| Storage                                                   | Provide storage that meets the application and database performance requirements.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Networking                                                | * Deploy workloads in an isolated environment and enforce information flow policies. \n * Provide secure, encrypted connectivity to the cloud’s private network for management purposes. \n * Distribute incoming application requests across available compute resources. \n * Provide public and private DNS resolution to support the use of hostnames instead of IP addresses.                                                                                                                                                                                                                                                                                                                             |
| Security                                                  | * Ensure that all operator actions are run securely through a bastion host. \n * Protect the boundaries of the application against denial-of-service and application-layer attacks. \n * Encrypt all application data in transit and at rest to protect it from unauthorized disclosure. \n * Encrypt all backup data to protect from unauthorized disclosure. \n * Encrypt all security data (operational and audit logs) to protect from unauthorized disclosure. \n * Encrypt all data that uses customer-managed keys to meet regulatory compliance requirements for more security and customer control. \n * Protect secrets through their entire lifecycle and secure them using access control measures. |
| Resiliency                                                | * Support application availability targets and business continuity policies. \n * Provide highly available compute, storage, network, and other cloud services to handle application load and performance requirements. \n * Backup application data to enable recovery if unplanned outages occur. \n * Provide highly available storage for security data (logs) and backup data.                                                                                                                                                                                                                                                                                                                             |
| Service Management                                        | * Monitor system and application health metrics and logs to detect issues that might impact the availability of the application. \n * Generate alerts/notifications about issues that might impact the availability of applications to trigger appropriate responses to minimize downtime. \n * Monitor audit logs to track changes and detect potential security problems. \n * Provide a mechanism to identify and send notifications about issues that are found in audit logs.                                                                                                                                                                                                                              |
| {: caption="Requirements" caption-side="bottom"} |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |

The above table lists all the requirements considered part of reference architecture.

## Components
{: #components}

The common solution components that are listed in the following table are those components that are needed for both scenarios.

| Aspects                                                 | Architecture components                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | How the component is used                                                                                                                                 |
| ------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Compute                                                 | [VPC VSIs](https://cloud.ibm.com/vpc-ext/provision/vs){: external}, [IBM Power Virtual Server](/docs/power-iaas?topic=power-iaas-getting-started)                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Oracle Database, other non-Oracle application that is hosted on cloud Non-Oracle workloads that are run on x86 (VPC) and Oracle workloads runs on PowerVS |
| Storage                                                 | Tier 1 Power Virtual Server Storage                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Database servers storage                                                                                                                                  |
|                                                         | Tier 3 Power Virtual Server Storage                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Archive or backup storage                                                                                                                                 |
|                                                         | [Cloud Object Storage](/docs/cloud-object-storage?topic=cloud-object-storage-about-cloud-object-storage)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Backups and Logs (application, operational, and audit logs)                                                                                               |
|                                                         | [Block Storage](/docs/vpc?topic=vpc-block-storage-about#vpc-storage-encryption)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Block storage for VPC VSI images on x86 workloads                                                                                                         |
| Networking                                              | [Transit Gateway](/docs/transit-gateway?topic=transit-gateway-about)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | Connects across VPCs and PowerVS                                                                                                                          |
|                                                         | [VPC Virtual Private Network (VPN)](/docs/iaas-vpn?topic=iaas-vpn-getting-started)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Remote access to manage resources in a private network                                                                                                    |
|                                                         | [Virtual Private Gateway &amp; Virtual Private Endpoint (VPE)](/docs/vpc?topic=vpc-about-vpe)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Private network access to Cloud Services, for example Key Protect, Cloud Object Storage, and so on.                                                       |
|                                                         | [VPC Application Load Balancers](/docs/vpc?topic=vpc-load-balancers)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | Application Load Balancing for web servers, app servers, and database servers                                                                             |
|                                                         | [Public Gateway](/docs/vpc?topic=vpc-about-public-gateways)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | For web server access to the internet                                                                                                                     |
|                                                         | [Cloud Internet Services (CIS)](/docs/cis?topic=cis-getting-started)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | Public Load balancing of web server traffic across zones in the region                                                                                    |
|                                                         | [DNS Services](/docs/dns-svcs?topic=dns-svcs-about-dns-services)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | Domain Name System (DNS) for Domain name resolution                                                                                                       |
| Security                                                | [IAM](/docs/account?topic=account-cloudaccess)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | IBM Cloud Identity & Access Management                                                                                                                    |
|                                                         | [BYO Bastion Host on VPC VSI with PAM SW](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-bastion-tutorial-teleport)                                                                                                                                                                                                                                                                                                                                                                                                                                     | Remote access for administrative functions with Privileged Access Management                                                                              |
|                                                         | [Virtual Private Clouds (VPCs), Subnets, Security Groups, ACLs](/docs/vpc?topic=vpc-getting-started) Isolated PowerVS LPARs                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Network Segmentation/Isolation for Power VS by using PVS subnets.                                                                                         |
|                                                         | [Cloud Internet Services (CIS)](/docs/cis?topic=cis-getting-started)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | Public DDoS protection and Web App Firewall                                                                                                               |
|                                                         | [Key Protect](/docs/key-protect) or [HPCS](/docs/hs-crypto?topic=hs-crypto-get-started)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | HSM and Key Management Service (KYOK)                                                                                                                     |
|                                                         | [Secrets Manager](https://cloud.ibm.com/catalog/services/secrets-manager){: external}                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | Certificate and Secrets Management                                                                                                                        |
|                                                         | Firewall:<br />[Fortigate](https://cloud.ibm.com/catalog/content/ibm-fortigate-AP-HA-terraform-deploy-5dd3e4ba-c94b-43ab-b416-c1c313479cec-global){: external}<br />[Juniper vSRX](https://cloud.ibm.com/catalog/content/juniper-vsrx-catalog-deploy-1.4-dc1e707c-33dd-4321-b2a5-c22dbf0dd0ee-global){: external}<br />[Checkpoint Cloud Guard](https://cloud.ibm.com/catalog/content/checkpoint-iaas-gw-ibm-vpc-1.0.7-9ed8dbde-2931-45f5-a7a7-0c90ce0d2686-global){: external}<br />[Palo Alto](https://cloud.ibm.com/catalog/content/ibmcloud-vmseries-1.9-6470816d-562d-4627-86a5-fe3ad4e94b30-global){: external} | IPS/IDS protection at all ingress/egress points Unified Threat Management (UTM) Firewall                                                                  |
| Service Management (Observability)                      | [IBM Cloud Monitoring](/docs/monitoring?topic=monitoring-about-monitor)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | Apps and operational monitoring                                                                                                                           |
|                                                         | [IBM Cloud Logs](/docs/cloud-logs?topic=cloud-logs-getting-started)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Apps and operational logs                                                                                                                                 |
|                                                         | [IBM Cloud Logs](/docs/cloud-logs?topic=cloud-logs-getting-started)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Audit logs                                                                                                                                                |
| {: caption="Components" caption-side="bottom"} 

As mentioned earlier, the [Architecture Design Framework](/docs/architecture-framework?topic=architecture-framework-intro) is used to guide and determine the applicable aspects and domains for which architecture decisions need to be made based on customer requirements. The following sections contain the considerations and architecture decisions for the aspects and domains that are contained in the PowerVS common elements for both Oracle Resiliency solution patterns.
