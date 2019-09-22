---
title: Network Storage - Notes Summary
author: Raymond
layout: post
date: 2019-09-22T07:50:26-04:00
categories:
  - Tech
---
Finished the book [Network Storage by James O'Reilly](https://www.goodreads.com/book/show/33533945-network-storage). It's not an area I'm familiar with. I read it pretty fast, took notes and now I'm coming back to sort things out.
Official and accurate definitions could be found on Wimipedia. I'll just describe some terms in my own words.

#DAS, NAS and SAN

##[DAS - Direct-attached Storage](https://en.wikipedia.org/wiki/Direct-attached_storage)
A drive locally attached. Like desktop hard drive

##[SAN - Storage Area Network](https://en.wikipedia.org/wiki/Storage_area_network)
Storage is in a dedicated device. The hosts access the storage through network and via a controller device. The storage usually provides block storage. File system metadata is stored in LUN(Logical Unit, on SAN storage device side) Implemenation include FC(Fiber Channel), SCSI, FCoE(Fiber Channel over Ethernet), iSCSI(Internet SCSI) etc.

##[NAS - Network-attached Storage]
Also called 'Filer'. A file service attached to the network. Hosts access its data in a file level. e.g. NFS, SMB etc. File system metadata is stored on the filer.

#SCSI/SAS/SATA and beyond

##[SCSI - Small Computer System Interface](https://en.wikipedia.org/wiki/SCSI)
A set of standards for physically connecting and transferring data between computers and peripheral devices. I view it as a combination of command set and physical bus interfaces. SATA and SAS are two branches (enterprise and commodity) of SCSI.

##[SAS - Serial Attached SCSI](https://en.wikipedia.org/wiki/Serial_Attached_SCSI)
Deemed to be enterprise-grade, reliable, expensive and usually with less capacity. 15,000 rpm is its limit.

##[SATA - Serial AT Attachment](https://en.wikipedia.org/wiki/Serial_ATA)
Much cheaper and less reliable.

##[PCIe - Peripheral Component Interconnect Express](https://en.wikipedia.org/wiki/PCI_Express)
A high-speed serial computer expansion bus standard, designed to replace the older PCI, PCI-X and AGP bus standards.

##[NVMe - Non-Volatile Memory Express](https://en.wikipedia.org/wiki/NVM_Express)
An open logical device interface specification for accessing non-volatile storage media attached via a PCI Express (PCIe) bus. NVMe is a queue-based IO solution, without the complex sequence of operations that command-driven interfaces like SCSI require. NVMe consolidates interrupts, flattens the whole file stack, and uses queues to communicate directly with drives.

#SSD (Solid-State Drive)

##AFA - All-Falsh Arrays

##Important characters of SSD

  1. Erase and Write on cell level
  2. Its wear-out issue is already solved by better technology and over provisioning
  3. Copy-on-write feature/problem. Unlike HDD, the new data goes into a different physical block to the old data that is being “erased”. Need to take care of the old block for security reason in multi-tenancy environment.

#Object Storage

[Object Storage](https://en.wikipedia.org/wiki/Object_storage) manages data as objects.

#Comparison of SAN/NAS/Object Storage

Feature list of each:

SAN: 
  1. Well established in enterprise IT

NAS:
  1. Mapped to OS instruction set well

Object Storage:
  1. Deduplication
  2. Not rely on RAID
  3. Scability
  4. No long large drive rebuild time
  5. Use cheap COTS components
  6. (Con)Object storage has back-end traffic that tends to saturate networks
  7. (Con)REST is also less flexible in append and insert type operations.

#Network

##[RDMA](https://en.wikipedia.org/wiki/Remote_direct_memory_access)
 A concept that allows two computers to share memory spaces over networks. The idea is to have one computer send packets of data directly into application buffers in another, so bypassing layers of protocol stacks and complicated buffer shuffling.  RDMA bypasses a stack (including TCP, IP and Network Device) which saves CPU around 30%. But NICs for RDMA are expensive.

##RoCE vs. iWARP

Two techs are competing in RDMA: RoCE (RDMA over Converged Ethernet) and iWARP (Internet Wide Area RDMA Protocol).

RoCE:

  1. RoCE uses Converged Ethernet, which requires expensive switches.

iWARP:

  1. Transfer over Internet Protocol networks

#RAID

Facts and future of RAID:

  1. RAID 5, often built around 6-drive sets, is typically only 85% capacity efficient due to the use of parity, while RAID 6 drops to 67%. With a complete copy needed as a minimum for disaster recovery, the real numbers are 40 and 33%.

  2. RAID 5/6 will be obsolecent soon.

#Add-on values of storage

  1. Deduplication
  2. Compression
  3. Encryption

#VSAN (Virtual SAN)

Making virtual SANs perform well has proven to be a bit of a challenge, since network bandwidth is at a premium in today’s data centers, and typically the network effectively throttles the drives, but faster networks including RDMA and real-time compression may fix that problem or at least reach a level of adequacy. Longer term, the idea of the virtual SAN is itself challenged by the arrival of NVDIMM memories such as ReRAM and X-Point. Most likely all three layers (DRAM, NVDIMM, and NVMe drives) will be shareable across the network,

#Misc

## [NVDIMM - Non-Volatile DIMM](https://en.wikipedia.org/wiki/NVDIMM)
Put flash on DIMMs, making the whole memory recoverable from power off.

##[HMC - Hybrid Memory Cube]()
An archgitecture to remove the separation between DRAM and CPU by mounting them all on one module(packing the CPU and DRAM on a silicon substrate as a module, the speed of the DRAM interface goes way up, while power goes way down since the distance between devices is so small that driver transistors are no longer needed.), removing the high-power off chip drivers and thus speeding up the DRAM significantly while dropping power.
