---
copyright:
  years: 2024
lastupdated: "2024-05-10"

subcollection: pattern-oracle-disaster-recovery-on-powervs

keywords:
---
{{site.data.keyword.attribute-definition-list}}

# Power Virtual Server Deployment
{: #powervs-da}

## Deployment Overview:
{: #deploy-overview}
This option involves deploying the "Power Virtual Server with VPC landing zone" Deployable Architecture (DA) in a multi-regional configuration, specifically in two distinct geographic regions. The deployment will be based on an existing DA template, followed by a series of manual customizations to tailor the setup to the specific requirements of your environment.

Deployment Steps and Customization:

1. Initial Deployment:

   * Deploy the "Power Virtual Server with VPC landing zone" DA twice: once in each target region (Region A and Region B). Utilize the provided documentation for guidance, available at [IBM Deployable Reference Architectures](https://cloud.ibm.com/docs/deployable-reference-architectures?topic=deployable-reference-architectures-deploy-arch-ibm-pvs-inf-full-stack).
2. Manual Initial Customization:

   * In Region B, after ensuring that no active connections depend on it, remove the existing transit gateway (you will need to remove the VPC and PowerVS connections first) to streamline network management and resource optimization.
   * In region A, designate the remaining transit gateway as global, enhancing its role in facilitating inter-region connectivity for your Power Virtual Server workspaces and VPCs.
   * Optionally, in both Regions (A & B), add additional VPCs/subnets/ACLs/security groups to expand your network infrastructure and cater to your specific needs.
   * Reconnect any Power Virtual Server workspaces and VPCs that were previously disconnected (in region B) to the global transit gateway established in Region A. This step ensures a unified and fully connected network across all regions.
   * Deploy the necessary number of PowerVS LPARs (Logical Partitions) in each region, ensuring they are configured according to your application requirements.

## Additional Manual Networking Customization:
{: #deploy-networking}
* Firewall Appliance Configuration : Configure firewalls according to customer preferences and security requirements, ensuring that the chosen firewall solution aligns with the intended level of protection and access control policies. Also ensure the proper routing rules are configured in the VPCs (egress and ingress) but also on the customer on premise router to steer traffic to the chosen firewall solution.
* DR Networking Configuration : Implement customized solutions for disaster recovery networking, which includes configuring VPC routing rules, setting up appropriate firewall rules, and any other network configurations required to support your DR strategy.
* DNS and Networking Customization : Adjust DNS settings and further configure the networking aspects of the environment based on customer inputs. This will likely involve making specific assumptions and decisions to align with the unique requirements of the customer's infrastructure.

## Oracle Deployment and customization:
{: #deploy-oracle}
The initial landing zone deployment described above does not encompass:

* Oracle Deployment : The setup and configuration of Oracle environments, including any necessary databases, applications, or middleware components.
* Oracle DR and RAC Configuration and Deployment : Designing and deploying Oracle High Availability (HA) solutions, Data Guard configurations for disaster recovery, Real Application Clusters (RAC), or other Oracle HA features as specified by Oracle best practices.
* Oracle Backup, HA and DR configuration and strategy : Establishing backup strategies, disaster recovery plans, and high availability setups specifically tailored to Oracle systems.
* Customer-Specific Networking and Firewall Setup : Custom network configurations and firewall settings that go beyond the standard configurations provided in the DA template to meet precise customer requirements or preferences.

This option is designed for customers who need a scalable, multi-region infrastructure with the flexibility of manual customizations post the initial deployment of the base DA. It allows for adapting various components, such as networking and security, to better suit individual business needs after the foundational architecture has been established.
