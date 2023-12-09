---
copyright:
  years: 2023
lastupdated: "2023-11-28"

subcollection: <repo-name>

keywords:

---

\# Compute design decisions

{: \#compute-design}

Power Virtual Servers are available with flexible hardware configurations on both Power S922 and E980. It is permitted to define a custom size of the IBM Power Virtual Server to use for Oracle Database instance.

The Flexibility of IBM Power Systems Virtual Servers hardware capability includes:

-   Cores (CPU)
-   Memory (RAM)
-   Data volume size / volume type / performance tier
-   Network interfaces
-   Power virtual server Host Pinning Policy (soft or hard)
-   Power virtual server Host CPU Binding (dedicated or shared)
-   Reserved Capacity via Shared Processor Pool Option

The IBM Power Systems Virtual Server environment consists of SAN Storage, Power Systems servers, PowerVM Hypervisor, and AIX Operating Systems that are certified for Oracle DB (12c, 18c, 19c) including RAC. IBM has collaborated with Oracle through our longstanding joint technical partnership to ensure IBM Power Systems Virtual Server meets the current requirements of a certified and supported stack. The environment is considered supported under these requirements as long as it runs a stack as described above. In addition, since the environment is built on LPARs, it is consistent with Oracle's current hard partitioning guidelines, as long as LPM is not used with the LPARs running Oracle DB. Oracle licensing is always based on the contract between the customer and Oracle.

Oracle publishes their current DB certifications of the PowerVM hypervisor, its features, AIX 7.1, 7.2 & 7.3, and confirms support of these features here: https://www.oracle.com/database/technologies/virtualization-matrix.html.

When creating the PowerVS LPARS (also referred to as instances and VMs) there are several considerations regarding processor mode, processor pools, pinning and affinity.

a) There is an option to select between dedicated, capped shared, or uncapped shared processor mode for the virtual CPUs (vCPUs). Dedicated cores are recommended for Production Oracle Disaster Recovery workloads. Uncapped shared cores are used for Dev/Test workloads. Dev/Test Workloads can be configured with as low as 0.25 cores. Select the processor mode and number of cores based on your Oracle licensing terms.

b) LPARs can be deployed in a shared processor pool, which has a placement policy option of anti-affinity to ensure both shared processor pools are on different physical server. Dedicated cores are very expensive as opposed to shared capped cores. The recommendation is to select the processor mode that matches the requirement (dedicated, shared capped, shared uncapped). Clients will most probably run production with shared capped in a shared pool unless they want dedicated cores.

c) Users can also select the appropriate pinning option (Hard/Soft/None) from the Virtual server pinning list. Select the Hard pin option to restrict the movement of a LPAR to a different host. It is recommended to select Hard pin for LPARs running Oracle databases to control the use of licensed cores and to prevent LPM activity.

d) Server placement groups provide you control over the host or server on which a new virtual machine (VM) is placed. By using server placement groups, you can apply an affinity or anti-affinity policy to each VM instance within a server placement group. After you create a placement group, you can provision a new VM instance in the placement group. When you set a placement group with an affinity policy, all VMs in that placement group are launched on the same server. When you set a placement group with an anti-affinity policy, all VMs in that placement group are launched on different servers.

\# Compute Architecture Decisions

| **Aspects** | **Domains**          | **Requirements**                                                                                              | **Chosen Service** | **Decisions / Rationale**                                                                                                                                                                                                    |
|-------------|----------------------|---------------------------------------------------------------------------------------------------------------|--------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Compute     | Management Workloads | AIX LPARS for deployment of Management tools. Consider Oracle DG management tool stack and other tools needed | PowerVS LPAR       | For DG implementation and Oracle Database management tools                                                                                                                                                                   |
| Compute     | Workload             | Actual Oracle Workload images of LPARS.                                                                       | PowerVS LPAR       | [For Oracle DG Database Standard DG implementation, follow DG guidance](https://docs.oracle.com/en/database/oracle/oracle-database/19/haovw/oracle-data-guard-best-practices.html#GUID-C3A78B07-6584-4380-8D53-E5B831A5894C) |
| Compute     | VPC                  | To Host Common Edge, Management, and non-Oracle Workloads on this environment                                 | IBM VPC VSI        | X86 and connect to all cloud services                                                                                                                                                                                        |
