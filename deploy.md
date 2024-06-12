---
copyright:
  years: 2024
lastupdated: "2024-05-10"

subcollection: pattern-oracle-disaster-recovery-on-powervs

keywords:
---
{{site.data.keyword.attribute-definition-list}}

# Deploying a Power Virtual Server deployable architecture

{: #powervs-da}

This guide outlines deploying a Power Virtual Server with VPC landing zone deployable architecture in a multi-regional configuration, specifically in two distinct regions for a Oracle Database disaster recovery scenario. The deployment is based on an existing deployable architecture template, as well as a series of manual customizations to tailor the setup to the specific requirements for your environment.

This is designed for customers who need a scalable, multi-region infrastructure with the flexibility of manual customizations post the initial deployment of the base Deployable Architecture. It allows for adapting various components, such as networking and security, to better suit individual business needs after the foundational architecture has been established.

Installation and configuration of Oracle Data Guard is not described in this section.

## Planning for the landing zone deployable architectures

Before you begin the deployment of a landing zone deployable architecture, make sure that you understand and meet the prerequisites.

### Confirm your IBM Cloud settings

Complete the following steps before you deploy the VPC landing zone deployable architecture.

1. Confirm or set up an IBM Cloud account:
   Make sure that you have an IBM Cloud Pay-As-You-Go or Subscription account:
   * If you don't have an IBM Cloud account,** **[create one](https://cloud.ibm.com/docs/account?topic=account-account-getting-started).
   * If you have a Trial or Lite account,** **[upgrade your account](https://cloud.ibm.com/docs/account?topic=account-upgrading-account).
2. Configure your IBM Cloud account:
   1. Log in to** **[IBM Cloud](https://cloud.ibm.com/) with the IBMid you used to set up the account. This IBMid user is the account owner and has full IAM access.
   2. [Complete the company profile](https://cloud.ibm.com/docs/account?topic=account-contact-info) and contact information for the account. This profile is required to stay in compliance with IBM Cloud Financial Services profile.
   3. Enable virtual routing and forwarding (VRF) and service endpoints by creating a support case. Follow the instructions in** **[enabling VRF and service endpoints](https://cloud.ibm.com/docs/account?topic=account-vrf-service-endpoint&interface=ui#vrf).

### Set the IAM permissions

Set up account access (Cloud Identity and Access Management (IAM)):
Create an IBM Cloud API key. The user who owns this key must have the Administrator role.

Service ID API keys are not supported for the Red Hat OpenShift Container Platform on VPC landing zone deployable architecture.

For compliance with IBM Cloud Framework for Financial Services: Require users in your account to use multifactor authentication (MFA).
Set up access groups.

User access to IBM Cloud resources is controlled by using the access policies that are assigned to access groups. For IBM Cloud Financial Services validation, do not assign direct IAM access to any IBM Cloud resources.

Select All Identity and Access enabled services when you assign access to the group.

### Verify access roles

IAM access roles are required to install this deployable architecture and create all the required elements.

You need the following permissions for this deployable architecture:

* Create services from IBM Cloud catalog.
* Create and modify IBM Cloud VPC services, virtual server instances, networks, network prefixes, storage volumes, SSH keys, and security groups of this VPC.
* Create and modify IBM Cloud direct links and IBM Cloud Transit Gateway.
* Access existing Object Storage services.

For information about configuring permissions, contact your IBM Cloud account administrator.

### Create an SSH key

Make sure that you have an SSH key that you can use for authentication. This key is used to log in to all virtual server instances that you create. For more information about creating SSH keys, see SSH keys.

### Additional information

Please follow the link for setting up your environment for [deployable architecture](https://cloud.ibm.com/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-plan)

## Setting up the initial deployment

{: #initial-deployment-power-vs}

Before you start creating environment, ensure you have above prequisite completed and you have configured primary and secondary region.

Deploy the Power Virtual Server with VPC landing zone deployable  architecture (DA) twice: once in each target region (Region A and Region B). Utilize the provided documentation for guidance, available at [IBM Deployable Reference Architectures](https://cloud.ibm.com/docs/deployable-reference-architectures?topic=deployable-reference-architectures-deploy-arch-ibm-pvs-inf-full-stack).
.

## Initial Customization

{: #initial-customization}

* Selection two regions for your deployment A and B.
* In region A, designate the transit gateway as global, allowing inter-region connectivity for your Power Virtual Server workspaces and VPCs.
* In Region B, after ensuring that no active connections depend on it, remove the existing transit gateway (you will need to remove the VPC and PowerVS connections first).
* Reconnect any Power Virtual Server workspaces and VPCs that were previously disconnected (in region B) to the global transit gateway established in Region A. This step ensures a unified and fully connected network across all regions.
* Deploy the necessary number of PowerVS LPARs (Logical Partitions) in each region, ensuring they are configured according to your application requirements.
* Optionally, in both Regions (A & B), add additional VPCs/subnets/ACLs/security groups to expand your network infrastructure and cater to your specific needs.

## Manual Networking Configuration:

{: #deploy-networking}

Please consider these additional changes as a part of your networking design and deployment.

* Firewall Appliance Configuration : Configure firewalls according to customer preferences and security requirements, ensuring that the chosen firewall solution aligns with the intended level of protection and access control policies. Also ensure the proper routing rules are configured in the VPCs (egress and ingress) but also on the customer on premise router to steer traffic to the chosen firewall solution.
* DR Networking Configuration : Implement customized solutions for disaster recovery networking, which includes configuring VPC routing rules, setting up appropriate firewall rules, and any other network configurations required to support your DR strategy.
* DNS and Networking Customization : Adjust DNS settings and further configure the networking aspects of the environment based on customer inputs. This will likely involve making specific assumptions and decisions to align with the unique requirements of the customer's infrastructure.

## Oracle Deployment and customization:

{: #deploy-oracle}

This section covers how an end user prepares their Power Virtual server environment for Oracle Database.
Here is the list of additional tasks required for Oracle Database configuration in IBM Power Virtual Server environment.

* Oracle Deployment : The setup and configuration of Oracle environments, including any necessary databases, applications, or middleware components.
* Oracle DR and RAC Configuration and Deployment : Designing and deploying Oracle High Availability (HA) solutions, Data Guard configurations for disaster recovery, Real Application Clusters (RAC), or other Oracle HA features as specified by Oracle best practices.
* Oracle Backup, HA and DR configuration and strategy : Establishing backup strategies, disaster recovery plans, and high availability setups specifically tailored to Oracle systems.
* Customer-Specific Networking and Firewall Setup : Custom network configurations and firewall settings that go beyond the standard configurations provided in the DA template to meet precise customer requirements or preferences.
