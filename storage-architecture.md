---
copyright:
  years: 2023
lastupdated: "2023-11-28"

subcollection: <repo-name>

keywords:
---
**Storage Architecture Decisions**

| **Aspects** | **Key Domain** | **Storage Domain** | **Requirements**                            | **Chosen Service** | **Decisions / Rationale**                                                                              |
| ----------------- | -------------------- | ------------------------ | ------------------------------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------------ |
| Storage           | Management Tools     | Primary Storage          | To host Oracle Management tools                   | Tier 1 Storage           | As per Oracle recommendations, for Management workloads use Tier 1                                           |
| Storage           | Management Tools     | Backup Storage           | Used for Oracle Management backup tools with RMAN | Tier 3                   | COS and/or Tier 3 storage                                                                                    |
| Storage           | Workloads            | Primary Storage          | For Oracle DG DB                                  | Tier 1                   | Highest tier available As per Oracle recommendations, for Production and non-production workloads use Tier 1 |
| Storage           | Workloads            | Backup Storage           | For DB backup                                     | Tier 3                   | Backup of DB using RMAN                                                                                      |
| Storage           | Workloads            | Archive Storage          | For DB and LPAR archival                          | COS                      | Long term backup                                                                                             |
