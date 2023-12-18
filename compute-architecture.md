---
copyright:
  years: 2023
lastupdated: "2023-12-18"

subcollection: pattern-oracle-disaster-recovery-on-powervs

keywords:
---

# Architecture decisions for compute
{: #architecture-compute}

The following are the compute architecture decisions that are considered to host Power Virtual server LPARS.

Power Virtual Servers are available with flexible hardware configurations on both Power S922 and E980. You can define a custom size of the IBM Power Virtual Server to use for Oracle Database instance.


| Aspects | Domains    | Requirements                                                                                        | Chosen service | Decisions and rationale                                                                                                                                                                                          |
| ----------------- | -------------------- | ------------------------------------------------------------------------------------------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Compute           | Management Workloads | AIX LPARS for deployment of Management tools. Consider the Oracle DG management tool stack and other tools needed | PowerVS LPAR             | [For Oracle DG Database Standard DG implementation, follow DG guidance](https://docs.oracle.com/en/database/oracle/oracle-database/19/haovw/oracle-data-guard-best-practices.html#GUID-C3A78B07-6584-4380-8D53-E5B831A5894C) |
| Compute           | Workload             | Actual Oracle Workload images of LPARS.                                                                       | PowerVS LPAR             | [For Oracle DG Database Standard DG implementation, follow DG guidance](https://docs.oracle.com/en/database/oracle/oracle-database/19/haovw/oracle-data-guard-best-practices.html#GUID-C3A78B07-6584-4380-8D53-E5B831A5894C) |
| Compute           | VPC                  | To Host Common Edge, Management, and non-Oracle Workloads on this environment                                 | IBM VPC VSI              | X86 and connect to all cloud services                                                                                                                                                                                     |
{: caption="Table 1. Architecture decisions for compute" caption-side="bottom"}
