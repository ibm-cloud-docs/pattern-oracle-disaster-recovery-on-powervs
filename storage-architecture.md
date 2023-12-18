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

| Aspects | Key Domain | Storage domain | Requirements                            | Chosen service | Decisions or rationale                                                                              |
| ----------------- | -------------------- | ------------------------ | ------------------------------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------------ |
| Storage           | Management Tools     | Primary Storage          | To host Oracle Management tools                   | Tier 1 Storage           | Per Oracle recommendations, for Management workloads use Tier 1                                           |
| Storage           | Management Tools     | Backup Storage           | Used for Oracle Management backup tools with RMAN | Tier 3                   | Cloud Object Storage and/or Tier 3 storage                                                                                    |
| Storage           | Workloads            | Primary Storage          | For Oracle DG DB                                  | Tier 1                   | Highest tier available per Oracle recommendations, for production and nonproduction workloads use Tier 1 |
| Storage           | Workloads            | Backup Storage           | For DB backup                                     | Tier 3                   | Backup of DB by using RMAN                                                                                      |
| Storage           | Workloads            | Archive Storage          | For DB and LPAR archival                          | Cloud Object Storage                      | Long-term backup                                                                                             |
{: caption="Table 1. Architecture decsions for storage" caption-side="bottom"}
