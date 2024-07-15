---
copyright:
  years: 2023
lastupdated: "2023-12-18"

subcollection: pattern-oracle-disaster-recovery-on-powervs

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for security
{: #architecture-security}

The following sections summarize the security architecture decisions for resilient solutions.

| **Architecture decision**               | **Requirements**                                                                                                                                 | **Decision**                                                                                                                                             | **Rationale**                                                                                                                                                                                                                  |
|-----------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Primary Storage                         | Ability to encrypt system volumes with BYOK                                                                                                      | Key Protect / HPCS                                                                                                                                       | By default, storage is encrypted. Use IBM Key Protect / HPCS for dedicated key management service                                                                                                                              |
| Backup Storage & Archive Storage        | Ability to encrypt backups                                                                                                                       | Cloud Object Storage Block Storage                                                                                                                       | By default, all objects that are stored in IBM Cloud Object Storage are encrypted by using randomly generated keys and an all-or-nothing-transform (AONT)                                                                      |
| Oracle Data Encryption                  | Ability to encrypt Oracle data at rest                                                                                                           | Transparent Data Encryption (TDE)                                                                                                                        | TDE encrypts data on database storage media, such as tablespaces and datafiles, and on backup media                                                                                                                            |
| Workload                                | Ability to encrypt data while in transit, to server(s) and between the Oracle Client and DB servers and between PowerVS and any attached storage | HTTPs protocols (client to server) Inflight encryption for using TLS/SSL Oracle Native Network Encryption (NNE)                                          | Client to server encryption can be accomplished over HTTPs (SSL) Oracle DB supports SSL or NNE can be used between the Oracle Client and DB                                                                                    |
| Identity Access & Role Management (IDM) | Securely authenticate users for platform services and control access to resources consistently across IBM Cloud                                  | IBM Cloud IAM                                                                                                                                            | Use IAM access policies to assign users, service IDs, and trusted profiles access to resources within the IBM Cloud account                                                                                                    |
| Privileged Identity & Access Management | Privileged access management services for administrative purposes                                                                                | BYO Bastion host (or Privileged Access Gateway) with PAM SW deployed in Edge VPC 2FA(Two Factor Authentication)Authentication though IBM Security Verify | Securely access remote resources over the private network for management purposes; bastion accessed via SSH. Session recording, tracking all activities, successful or not, to note any potential threats                      |
| Core Network Protection                 | Strict separation of duties Isolated security zones between environments Isolated, private cloud environment                                     | Separate VPCs, Subnets, ACLs and Security Groups separating application from DB as well as PRD and non-PRD environments                                  | A design combination using: Separate VPCs (transit, management, workload) connected through transit gateway and, the use of edge firewall capabilities. Subnets, Security Groups and ACLs to create an Edge/Transit VPC design |
{: caption="Table 1. Architecture decisions for security" caption-side="bottom"}
