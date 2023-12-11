---
copyright:
  years: 2023
lastupdated: "2023-11-28"

subcollection: <repo-name>

keywords:
---
# Compute Architecture Decisions

| **Aspects** | **Domains**    | **Requirements**                                                                                        | **Chosen Service** | **Decisions / Rationale**                                                                                                                                                                                           |
| ----------------- | -------------------- | ------------------------------------------------------------------------------------------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Compute           | Management Workloads | AIX LPARS for deployment of Management tools. Consider Oracle DG management tool stack and other tools needed | PowerVS LPAR             | For DG implementation and Oracle Database management tools                                                                                                                                                                |
| Compute           | Workload             | Actual Oracle Workload images of LPARS.                                                                       | PowerVS LPAR             | [For Oracle DG Database Standard DG implementation, follow DG guidance](https://docs.oracle.com/en/database/oracle/oracle-database/19/haovw/oracle-data-guard-best-practices.html#GUID-C3A78B07-6584-4380-8D53-E5B831A5894C) |
| Compute           | VPC                  | To Host Common Edge, Management, and non-Oracle Workloads on this environment                                 | IBM VPC VSI              | X86 and connect to all cloud services                                                                                                                                                                                     |
