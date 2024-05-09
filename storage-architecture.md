---
copyright:
  years: 2023
lastupdated: "2023-12-18"

subcollection: pattern-oracle-disaster-recovery-on-powervs

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for storage

{: #architecture-storage}

The following sections summarize the architecture decisions for storage for the Oracle Disaster Recovery on IBM Power Virtual Servers pattern.

| **Architecture decision** | **Requirements**                                  | **Decision**   | **Rationale**                                                                                                |
|---------------------------|---------------------------------------------------|----------------|--------------------------------------------------------------------------------------------------------------|
| Management Tools          | To host Oracle Management tools                   | Tier 1 Storage | As per Oracle recommendations, for Management workloads use Tier 1                                           |
| Management Tools          | Used for Oracle Management backup tools with RMAN | Tier 3         | COS and/or Tier 3 storage                                                                                    |
| Workloads                 | For Oracle DG DB                                  | Tier 1         | Highest tier available As per Oracle recommendations, for Production and non-production workloads use Tier 1 |
| Workloads                 | For DB backup                                     | Tier 3         | Backup of DB using RMAN                                                                                      |
| Workloads                 | For DB and LPAR archival                          | COS            | Long term backup                                                                                             |

{: caption="Table 1. Architecture decisions for storage" caption-side="bottom"}
