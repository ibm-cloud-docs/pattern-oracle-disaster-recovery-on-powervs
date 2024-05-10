---
copyright:
  years: 2023
lastupdated: "2023-12-18"

subcollection: pattern-oracle-disaster-recovery-on-powervs

keywords:

---
{{site.data.keyword.attribute-definition-list}}
# Network design considerations
{: #design-network}

Networking is a key aspect of any cloud deployment that should not be overlooked. The network design should consider enterprise connectivity, latency, throughput, resiliency, and security requirements.

For security purposes, it is recommended to isolate production from nonproduction workloads as a best practice. In addition, it’s also a best practice to isolate by application tiers (for example application versus database). PowerVS enables the creation of logical isolation across separate LPARs. For more information, see [Reference network design and subnet considerations](/docs/power-iaas?topic=power-iaas-network-architecture-diagrams).

To provide isolation and centralized advanced security functions, the network design follows a “Hub and spoke” VPC model. The Edge VPC serves as the hub for which all ingress and egress traffic flows. The Edge is a virtual network (VPC) that acts as a central point of connectivity to on-premises network and all other VPC and PowerVS environments. To achieve network isolation, [one must consider the PowerVS subnet](/docs/power-iaas?topic=power-iaas-configuring-subnet) and the use of a virtual firewall in VPC used as a default gateway for the workloads. PowerVS is connected to the Edge (“Hub”) through a Transit Gateway that allows traffic routing between each of the PowerVS, VPC, and classic environments in the IBM Cloud account.

Enterprise connectivity might be needed to connect the Power Virtual Server environment to the client data centers. IBM Cloud Direct Link is typically used for this connectivity and if redundancy is needed, two direct links can be used in a resilient design.

Several network scenarios that are described in [Power VS networking](/docs/power-iaas?topic=power-iaas-network-architecture-diagrams) show you how to connect your PowerVS environment back to the enterprise as well as to VPC and IBM Cloud Classic.
