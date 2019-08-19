---
layout: default
title: Test
nav_order: 9
---

# Hyper-V backup modes
{: .no_toc }

Veeam Backup and Replication provides two different backup modes to process Hyper-V backups, both relying on the Microsoft VSS framework.
{: .fs-5 .fw-300 }


* **On-Host** backup mode, for which backup data processing is on the Hyper-V node hosting the VM, leveraging non transportable shadow copies by using software VSS provider.
* **Off-Host** backup mode, for which backup data processing is offloaded to another non clustered participating Hyper-V node, leveraging transportable shadow copies using Hardware VSS provider provided by the SAN storage vendor.


Backup mode availability is heavily depending on the underlying virtualization infrastructure, leaving Off-Host backup mode available only to protect virtual machines hosted on SAN storage volumes. It is important that the VSS Framework provided by the storagevendor is tested and certified to work with Microsoft Hyper-V Clusters. Intensive checks of vendor VSS provider during POC is highly recommended (Cluster environment).
{: .fs-5 .fw-300 }

Performance wise, since both backup modes are using the exact same Veeam transport services, the only differentiating factors will be the additional time requested to manage transportable snapshots (in favor of On-Host mode) and the balance between compute and backup resources consumption during backup windows (in favor of Off-Host mode).
{: .fs-5 .fw-300 }

When using Windows Server 2016 On-Host proxy mode is very fast and will reduce the amount of included components. The Veeam Agent will then allocate the performance for deduplication and compression on the hosts systems. Consider that when planning the Veeam Job design. Please be aware that you will need up to 2GB of RAM on the Hyper-V Host per running task (one task = backup of one virtual disk). This memory must be free during Backup, otherwise the Hyper-V Host will start paging, what will end up in a slow system at all.
{: .fs-5 .fw-300 }

**Backup modes selection matrix**
{: .fs-5 .fw-300 }


|              | **PRO**                                                                      |**CON**                                                               |
|:-------------|:-----------------------------------------------------------------------------|:---------------------------------------------------------------------|
|              | -Simplifies management                                                       |                                                                      |
|  On-Host     | -Does not depend on third party VSS provider                                 | -Requires additional resources from the hypervisors (CPU, Network IO                                                                                                | and RAM) during the backup window, for IO processing and optimization|
|              | -Does not require additional hardware, load is spread over all Hyper-V hosts |                                                                      |
|              | -Can be used on any Hyper-V infrastructures                                  |                                                                      |
|:-------------|:-----------------------------------------------------------------------------|:---------------------------------------------------------------------|

