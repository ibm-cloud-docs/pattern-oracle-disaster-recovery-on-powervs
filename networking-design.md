---
copyright:
  years: 2023
lastupdated: "2023-11-28"

subcollection: <repo-name>

keywords:
---
## Network Considerations

Networking is a key aspect of any cloud deployment that should not be overlooked. The network design should consider enterprise connectivity, latency, throughput, resiliency, and security requirements.

For security purposes, it is recommended to isolate production from non-production workloads as a best practice. In addition, it’s also a best practice to isolate by application tiers (e.g. application vs. database). PowerVS enables the creation of logical isolation across separate LPARs. [Reference network design and subnets considerations](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-network-architecture-diagrams) for additional information.

To provide isolation and centralize advanced security functions, the network design follows a “Hub and spoke” VPC model. The Edge VPC serves as the hub for which all ingress and egress traffic flows. The Edge is a virtual network (VPC) that acts as a central point of connectivity to on-premises network and all other VPC and PowerVS environments. In order to achieve network isolation, [one must consider PVS subnet consider the PowerVS subnet(s)](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-configuring-subnet) and the use of a virtual firewall in VPC used as a default gateway for the workloads. PowerVS is connected to the Edge (“Hub”) via a Transit Gateway which allows traffic routing between each of the PowerVS, VPC and classic environments in the IBM Cloud account.

Enterprise connectivity may be needed to connect the Power Virtual Server environment to the client data center(s). IBM Cloud Direct Link is typically used for this connectivity and if redundancy is needed, two direct links can be used in a resilient design.

There are several network scenarios described in [Power VS networking](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-network-architecture-diagrams) to connect your PowerVS environment back to the enterprise as well as to VPC and IBM Cloud Classic.
