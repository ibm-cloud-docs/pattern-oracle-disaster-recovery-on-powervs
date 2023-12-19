---
copyright:
  years: 2023
lastupdated: "2023-12-18"

subcollection: pattern-oracle-disaster-recovery-on-powervs

keywords:

---
# Compute design decisions
{: #compute-design}

Power Virtual Servers are available with flexible hardware configurations on both Power S922 and E980. You can define a custom size of the IBM Power Virtual Server to use for Oracle Database instance.

The Flexibility of IBM Power Systems Virtual Servers hardware capability includes:

- Cores (CPU)
- Memory (RAM)
- Data volume size, volume type, performance tier
- Network interfaces
- Power virtual server Host Pinning Policy (soft or hard)
- Power virtual server Host CPU Binding (dedicated or shared)
- Reserved capacity through a shared processor pool option

The IBM Power Systems Virtual Server environment consists of SAN Storage, Power Systems servers, PowerVM Hypervisor, and AIX Operating Systems that are certified for Oracle DB (12c, 18c, 19c) including RAC. IBM collaborated with Oracle through our longstanding joint technical partnership to ensure that IBM Power Systems Virtual Server meets the current requirements of a certified and supported stack. The environment is considered supported under these requirements if it runs a stack as described. In addition, since the environment is built on LPARs, it is consistent with Oracle's current hard partitioning guidelines, if LPM is not used with the LPARs running Oracle DB. Oracle licensing is always based on the contract between the customer and Oracle.

Oracle publishes their current DB certifications of the PowerVM hypervisor, its features, AIX 7.1, 7.2 & 7.3, and confirms support of these features here: https://www.oracle.com/database/technologies/virtualization-matrix.html.

When you create the PowerVS LPARS (also referred to as instances and VMs) there are several considerations regarding processor mode, processor pools, pinning, and affinity.

a) There is an option to select between dedicated, capped shared, or uncapped shared processor mode for the virtual CPUs (vCPUs). Dedicated cores are recommended for Production Oracle Disaster Recovery workloads. Uncapped shared cores are used for dev or test workloads. Dev or test workloads can be configured with as low as 0.25 cores. Select the processor mode and number of cores based on your Oracle licensing terms.

b) LPARs can be deployed in a shared processor pool, which has a placement policy option of anti-affinity to ensure that both shared processor pools are on different physical server. Dedicated cores are very expensive as opposed to shared capped cores. The recommendation is to select the processor mode that matches the requirement (dedicated, shared capped, shared uncapped). Clients will most probably run production with shared capped in a shared pool unless they want dedicated cores.

c) Users can also select the appropriate pinning option (Hard/Soft/None) from the Virtual server pinning list. Select the Hard pin option to restrict the movement of an LPAR to a different host. It is recommended that you select Hard pin for LPARs running Oracle databases to control the use of licensed cores and to prevent LPM activity.

d) Server placement groups provide you control over the host or server on which a new virtual machine (VM) is placed. By using server placement groups, you can apply an affinity or anti-affinity policy to each VM instance within a server placement group. After you create a placement group, you can provision a new VM instance in the placement group. When you set a placement group with an affinity policy, all VMs in that placement group are started on the same server. When you set a placement group with an anti-affinity policy, all VMs in that placement group are started on different servers.

