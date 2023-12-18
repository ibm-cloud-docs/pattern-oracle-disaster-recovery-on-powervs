---
copyright:
  years: 2023
lastupdated: "2023-12-18"

subcollection: pattern-oracle-disaster-recovery-on-powervs

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Overview
{: #overview}

The objective of this pattern is to provide a solution design for an Oracle Database deployment on IBM Power Virtual Server that meets disaster recovery requirements for enterprise workloads.

This pattern is intended to:

- Accelerate and simplify solution design by providing a standard IBM Power Virtual Server deployment architecture reference following the [IBM Architecture Framework](/docs/architecture-framework?topic=architecture-framework-intro).
- Provide a prescriptive, end-2-end enterprise-class solution design, with diagrams, component architecture decisions along with rationale for cloud component selection to meet enterprise requirements.
- Ensure that requirements can be met from a performance, system availability, and security perspective.

This pattern focuses on infrastructure deployment patterns.

The main objective of this document is to highlight and illustrate options for Oracle Database implementation on IBM Power Virtual Server for Disaster Recovery.

- Simplify the process of deploying Oracle Database on IBM Power Systems Virtual Servers.
- Ensure high performance, maximizing system availability, and a secure environment.
- Provide a prescriptive solution with full component choices and rationale for component selection.
- Illustrate reference implementation of Oracle Data Guard on Power Virtual Server environment across IBM regions.

This document is not an implementation document, but a pattern that provides guidance for Power VS architecture to be deployed across Power Virtual server environments and on premises Oracle DR requirements.
{: note}

We discuss two reference architectures for Oracle Database Disaster recovery by using Oracle Data Guard.

1. Oracle disaster recovery across IBM Cross-Region (across IBM Power Virtual Servers locations)
2. Oracle disaster recovery across IBM Power Virtual Server environment and Customer Data center (hybrid cloud scenario)
