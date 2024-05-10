---
copyright:
  years: 2023
lastupdated: "2023-12-18"

subcollection: pattern-oracle-disaster-recovery-on-powervs

keywords:
---
{{site.data.keyword.attribute-definition-list}}

# Storage design decisions
{: #storage-design}

The storage tiers in Power Systems Virtual Server are based on I/O operations per second (IOPS). It means that the performance of storage volumes is limited to the maximum number of IOPS based on volume size and storage tier.

For each Power Systems Virtual Server instance, you must select a Flash Storage from the IBM FS9000 series device. Tier 1 is set to 10 IOPS/GB or Tier 3 is set to 3 IOPS/GB.

For production databases the recommendation, for performance reasons, is always Tier 1 storage. However, for backup storage, Tier 3 can be chosen. An even more cost-effective solution would be on Cloud Object Storage.

## Shared storage
{: #shared-storage}

PowerVS provides a shared storage capability that is required for implementing Oracle Data Guard. Use shared storage and Oracle DG to ensure data consistency across nodes in DG.
