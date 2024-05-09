
---
copyright:
  years: 2024
lastupdated: "2024-05-10"

subcollection: pattern-oracle-disaster-recovery-on-powervs

keywords:
---
{{site.data.keyword.attribute-definition-list}}

# Power Virtual Server Deployment


## **Deployment Overview:**

This option involves deploying the "Power Virtual Server with VPC landing zone" Deployable Architecture (DA) in a multi-regional configuration, specifically in two distinct geographic regions. The deployment will be based on an existing DA template, followed by a series of manual customizations to tailor the setup to the specific requirements of your environment.

**Deployment Steps and Customization:**

1. **Initial Deployment:**
   * Deploy the "Power Virtual Server with VPC landing zone" DA twice: once in each target region (Region A and Region B). Utilize the provided documentation for guidance, available at [IBM Deployable Reference Architectures](https://cloud.ibm.com/docs/deployable-reference-architectures?topic=deployable-reference-architectures-deploy-arch-ibm-pvs-inf-full-stack).
2. **Manual Customization:**
   * In Region A, after ensuring that no active connections depend on it, terminate one of the existing transit gateways to streamline network management and resource optimization.
   * Designate the remaining transit gateway in Region A as global, enhancing its role in facilitating inter-region connectivity for your Power Virtual Server workspaces and VPCs.
   * In both Regions (A & B), create two additional VPCs to expand your network infrastructure and cater to your specific needs.
   * Deploy the necessary number of LPARs (Logical Partitions) in each region, ensuring they are configured according to your application requirements.
   * Reconnect any Power Virtual Server workspaces that were previously disconnected, as well as the newly created VPCs in both regions, to the global transit gateway established in Region A. This step ensures a unified and fully connected network across all regions.

## **Additional Customization Needs:**

* **Firewall Appliance Configuration** : Configure firewalls according to customer preferences and security requirements, ensuring that the chosen firewall solution aligns with the intended level of protection and access control policies.
* **DR Networking Configuration** : Implement customized solutions for disaster recovery networking, which includes configuring VPC routing rules, setting up appropriate firewall rules, and any other network configurations required to support your DR strategy.
* **DNS and Networking Customization** : Adjust DNS settings and further configure the networking aspects of the environment based on customer inputs. This will likely involve making specific assumptions and decisions to align with the unique requirements of the customer's infrastructure.

## **Out-of-Scope for Initial Deployment (To be Addressed Separately):**
The initial deployment will not encompass:

* **Oracle Deployment** : The setup and configuration of Oracle environments, including any necessary databases, applications, or middleware components.
* **Oracle DR and RAC Configuration and Deployment** : Designing and deploying Oracle High Availability (HA) solutions, Data Guard configurations for disaster recovery, Real Application Clusters (RAC), or other Oracle HA features as specified by Oracle best practices.
* **Oracle Backup and DR Configuration and HA** : Establishing backup strategies, disaster recovery plans, and high availability setups specifically tailored to Oracle systems.
* **Customer-Specific Networking and Firewall Setup** : Custom network configurations and firewall settings that go beyond the standard configurations provided in the DA template to meet precise customer requirements or preferences.

This option is designed for customers who need a scalable, multi-region infrastructure with the flexibility of manual customizations post the initial deployment of the base DA. It allows for adapting various components, such as networking and security, to better suit individual business needs after the foundational architecture has been established.
