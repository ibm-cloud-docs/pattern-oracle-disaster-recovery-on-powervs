---
copyright:
  years: 2023
lastupdated: "2023-11-28"

subcollection: <repo-name>

keywords:
---
# Storage design decisions

{: \#storage-design}

The storage tiers in Power Systems Virtual Server are based on I/O operations per second (IOPS). It means that the performance of storage volumes is limited to the maximum number of IOPS based on volume size and storage tier.

For each Power Systems Virtual Server instance, you must select a Flash storage from IBM FS9000 series devices [storage tiers](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-about-virtual-server#storage-tiers): Tier 1 is currently set to 10 IOPS/GB or Tier 3 is currently set to 3 IOPS/GB.

For production databases the recommendation, for performance reasons, is always Tier 1 storage. For backup storage, Tier 3 can be chosen, however an even more cost effective solution would be on Cloud Object Storage.

**Shared Storage**: PowerVS provides shared storage capability that is required for implementing Oracle Data Guard. Utilize shared storage and Oracle DG to ensure data consistency across nodes in DG.

**Note:** *For up-to-date specs, check* [*here*](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-about-virtual-server)
