System Architecture
=======================

Model Compute Nodes
----------------------

The Delta compute ecosystem is composed of five node types:

#. Dual-socket CPU-only compute nodes
#. Single socket 4-way NVIDIA A100 GPU compute nodes
#. Single socket 4-way NVIDIA A40 GPU compute nodes
#. Dual-socket 8-way NVIDIA A100 GPU compute nodes
#. Single socket 8-way AMD MI100 GPU compute nodes

The CPU-only and 4-way GPU nodes have 256 GB of RAM per node while the
8-way GPU nodes have 2 TB of RAM. The CPU-only node has 0.74 TB of local
storage while all GPU nodes have 1.5 TB of local storage.

Each socket contains an AMD 7763
processor:\ https://www.amd.com/system/files/documents/amd-epyc-7003-sb-hpc-esi-vps.pdf

Table. CPU Compute Node Specifications
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

========================= ===================
Specification             Value
Number of nodes           132
CPU                       AMD EPYC 7763
                          "Milan" (PCIe Gen4)
Sockets per node          2
Cores per socket          64
Cores per node            128
Hardware threads per core 1
Hardware threads per node 128
Clock rate (GHz)          ~ 2.45
RAM (GB)                  256
Cache (KB) L1/L2/L3       64/512/32768
Local storage (TB)        0.74 TB
========================= ===================

The AMD CPUs are set for 4 NUMA domains per socket (NPS=4).

Table. 4-way NVIDIA A40 GPU Compute Node Specifications
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+---------------------------+-----------------------------------------+
| Specification             | Value                                   |
+---------------------------+-----------------------------------------+
| Number of nodes           | 100                                     |
+---------------------------+-----------------------------------------+
| GPU                       | NVIDIA A40                              |
|                           | (`Vendor                                |
|                           | page <https://www.nvidi                 |
|                           | a.com/en-us/data-center/a40/#specs>`__) |
+---------------------------+-----------------------------------------+
| GPUs per node             | 4                                       |
+---------------------------+-----------------------------------------+
| GPU Memory (GB)           | 48 DDR6 with ECC                        |
+---------------------------+-----------------------------------------+
| CPU                       | AMD Milan                               |
+---------------------------+-----------------------------------------+
| CPU sockets per node      | 1                                       |
+---------------------------+-----------------------------------------+
| Cores per socket          | 64                                      |
+---------------------------+-----------------------------------------+
| Cores per node            | 64                                      |
+---------------------------+-----------------------------------------+
| Hardware threads per core | 1                                       |
+---------------------------+-----------------------------------------+
| Hardware threads per node | 64                                      |
+---------------------------+-----------------------------------------+
| Clock rate (GHz)          | ~ 2.45                                  |
+---------------------------+-----------------------------------------+
| RAM (GB)                  | 256                                     |
+---------------------------+-----------------------------------------+
| Cache (KB) L1/L2/L3       | 64/512/32768                            |
+---------------------------+-----------------------------------------+
| Local storage (TB)        | 1.5 TB                                  |
+---------------------------+-----------------------------------------+

| The AMD CPUs are set for 4 NUMA domains per socket (NPS=4).
| The A40 GPUs are connected via PCIe Gen4 and have the following
  affinitization to NUMA nodes on the CPU. Note that the relationship
  between GPU index and NUMA domain are inverse.

Table. 4-way NVIDIA A40 Mapping and GPU-CPU Affinitization
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

==== ==== ==== ==== ==== === ============ =============
     GPU0 GPU1 GPU2 GPU3 HSN CPU Affinity NUMA Affinity
GPU0 X    SYS  SYS  SYS  SYS 48-63        3
GPU1 SYS  X    SYS  SYS  SYS 32-47        2
GPU2 SYS  SYS  X    SYS  SYS 16-31        1
GPU3 SYS  SYS  SYS  X    PHB 0-15         0
HSN  SYS  SYS  SYS  PHB  X                
==== ==== ==== ==== ==== === ============ =============

Table Legend

X = Self
SYS = Connection traversing PCIe as well as the SMP interconnect between
NUMA nodes (e.g., QPI/UPI)
NODE = Connection traversing PCIe as well as the interconnect between
PCIe Host Bridges within a NUMA node
PHB = Connection traversing PCIe as well as a PCIe Host Bridge
(typically the CPU)
NV# = Connection traversing a bonded set of # NVLinks

Table. 4-way NVIDIA A100 GPU Compute Node Specifications
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+---------------------------+-----------------------------------------+
| Specification             | Value                                   |
+---------------------------+-----------------------------------------+
| Number of nodes           | 100                                     |
+---------------------------+-----------------------------------------+
| GPU                       | NVIDIA A100                             |
|                           | (`Vendor                                |
|                           | page <https://www.nvidia.com/en-u       |
|                           | s/data-center/a100/#specifications>`__) |
+---------------------------+-----------------------------------------+
| GPUs per node             | 4                                       |
+---------------------------+-----------------------------------------+
| GPU Memory (GB)           | 40                                      |
+---------------------------+-----------------------------------------+
| CPU                       | AMD Milan                               |
+---------------------------+-----------------------------------------+
| CPU sockets per node      | 1                                       |
+---------------------------+-----------------------------------------+
| Cores per socket          | 64                                      |
+---------------------------+-----------------------------------------+
| Cores per node            | 64                                      |
+---------------------------+-----------------------------------------+
| Hardware threads per core | 1                                       |
+---------------------------+-----------------------------------------+
| Hardware threads per node | 64                                      |
+---------------------------+-----------------------------------------+
| Clock rate (GHz)          | ~ 2.45                                  |
+---------------------------+-----------------------------------------+
| RAM (GB)                  | 256                                     |
+---------------------------+-----------------------------------------+
| Cache (KB) L1/L2/L3       | 64/512/32768                            |
+---------------------------+-----------------------------------------+
| Local storage (TB)        | 1.5 TB                                  |
+---------------------------+-----------------------------------------+

The AMD CPUs are set for 4 NUMA domains per socket (NPS=4).

Table. 4-way NVIDIA A100 Mapping and GPU-CPU Affinitization
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

==== ==== ==== ==== ==== === ============ =============
     GPU0 GPU1 GPU2 GPU3 HSN CPU Affinity NUMA Affinity
GPU0 X    NV4  NV4  NV4  SYS 48-63        3
GPU1 NV4  X    NV4  NV4  SYS 32-47        2
GPU2 NV4  NV4  X    NV4  SYS 16-31        1
GPU3 NV4  NV4  NV4  X    PHB 0-15         0
HSN  SYS  SYS  SYS  PHB  X                
==== ==== ==== ==== ==== === ============ =============

Table Legend

X = Self
SYS = Connection traversing PCIe as well as the SMP interconnect between
NUMA nodes (e.g., QPI/UPI)
NODE = Connection traversing PCIe as well as the interconnect between
PCIe Host Bridges within a NUMA node
PHB = Connection traversing PCIe as well as a PCIe Host Bridge
(typically the CPU)
NV# = Connection traversing a bonded set of # NVLinks

Table. 8-way NVIDIA A100 GPU Large Memory Compute Node Specifications
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+---------------------------+-----------------------------------------+
| Specification             | Value                                   |
+---------------------------+-----------------------------------------+
| Number of nodes           | 6                                       |
+---------------------------+-----------------------------------------+
| GPU                       | NVIDIA A100                             |
|                           | (`Vendor                                |
|                           | page <https://www.nvidia.com/en-u       |
|                           | s/data-center/a100/#specifications>`__) |
+---------------------------+-----------------------------------------+
| GPUs per node             | 8                                       |
+---------------------------+-----------------------------------------+
| GPU Memory (GB)           | 40                                      |
+---------------------------+-----------------------------------------+
| CPU                       | AMD Milan                               |
+---------------------------+-----------------------------------------+
| CPU sockets per node      | 2                                       |
+---------------------------+-----------------------------------------+
| Cores per socket          | 64                                      |
+---------------------------+-----------------------------------------+
| Cores per node            | 128                                     |
+---------------------------+-----------------------------------------+
| Hardware threads per core | 1                                       |
+---------------------------+-----------------------------------------+
| Hardware threads per node | 128                                     |
+---------------------------+-----------------------------------------+
| Clock rate (GHz)          | ~ 2.45                                  |
+---------------------------+-----------------------------------------+
| RAM (GB)                  | 2,048                                   |
+---------------------------+-----------------------------------------+
| Cache (KB) L1/L2/L3       | 64/512/32768                            |
+---------------------------+-----------------------------------------+
| Local storage (TB)        | 1.5 TB                                  |
+---------------------------+-----------------------------------------+

The AMD CPUs are set for 4 NUMA domains per socket (NPS=4).

Table. 8-way NVIDIA A100 Mapping and GPU-CPU Affinitization
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

+------+------+------+------+------+------+------+------+------+-----+--------------+---------------+
|      | GPU0 | GPU1 | GPU2 | GPU3 | GPU4 | GPU5 | GPU6 | GPU7 | HSN | CPU Affinity | NUMA Affinity |
+------+------+------+------+------+------+------+------+------+-----+--------------+---------------+
| GPU0 | X    | NV12 | NV12 | NV12 | NV12 | NV12 | NV12 | NV12 | SYS | 48-63        | 3             |
+------+------+------+------+------+------+------+------+------+-----+--------------+---------------+
| GPU1 | NV12 | X    | NV12 | NV12 | NV12 | NV12 | NV12 | NV12 | SYS | 48-63        | 3             |
+------+------+------+------+------+------+------+------+------+-----+--------------+---------------+
| GPU2 | NV12 | NV12 | X    | NV12 | NV12 | NV12 | NV12 | NV12 | SYS | 16-31        | 1             |
+------+------+------+------+------+------+------+------+------+-----+--------------+---------------+
| GPU3 | NV12 | NV12 | NV12 | X    | NV12 | NV12 | NV12 | NV12 | SYS | 16-31        | 1             |
+------+------+------+------+------+------+------+------+------+-----+--------------+---------------+
| GPU4 | NV12 | NV12 | NV12 | NV12 | X    | NV12 | NV12 | NV12 | SYS | 112-127      | 7             |
+------+------+------+------+------+------+------+------+------+-----+--------------+---------------+
| GPU5 | NV12 | NV12 | NV12 | NV12 | NV12 | X    | NV12 | NV12 | SYS | 112-127      | 7             |
+------+------+------+------+------+------+------+------+------+-----+--------------+---------------+
| GPU6 | NV12 | NV12 | NV12 | NV12 | NV12 | NV12 | X    | NV12 | SYS | 80-95        | 5             |
+------+------+------+------+------+------+------+------+------+-----+--------------+---------------+
| GPU7 | NV12 | NV12 | NV12 | NV12 | NV12 | NV12 | NV12 | X    | SYS | 80-95        | 5             |
+------+------+------+------+------+------+------+------+------+-----+--------------+---------------+
| HSN  | SYS  | SYS  | SYS  | SYS  | SYS  | SYS  | SYS  | SYS  | X   |              |               |
+------+------+------+------+------+------+------+------+------+-----+--------------+---------------+

Table Legend

X = Self
SYS = Connection traversing PCIe as well as the SMP interconnect between
NUMA nodes (e.g., QPI/UPI)
NODE = Connection traversing PCIe as well as the interconnect between
PCIe Host Bridges within a NUMA node
PHB = Connection traversing PCIe as well as a PCIe Host Bridge
(typically the CPU)
NV# = Connection traversing a bonded set of # NVLinks

Table. 8-way AMD MI100 GPU Large Memory Compute Node Specifications
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+---------------------------+-----------------------------------------+
| Specification             | Value                                   |
+---------------------------+-----------------------------------------+
| Number of nodes           | 1                                       |
+---------------------------+-----------------------------------------+
| GPU                       | AMD MI100                               |
|                           | (`Vendor                                |
|                           | page <https://www.amd.com/en/products/  |
|                           | server-accelerators/instinct-mi100>`__) |
+---------------------------+-----------------------------------------+
| GPUs per node             | 8                                       |
+---------------------------+-----------------------------------------+
| GPU Memory (GB)           | 32                                      |
+---------------------------+-----------------------------------------+
| CPU                       | AMD Milan                               |
+---------------------------+-----------------------------------------+
| CPU sockets per node      | 2                                       |
+---------------------------+-----------------------------------------+
| Cores per socket          | 64                                      |
+---------------------------+-----------------------------------------+
| Cores per node            | 128                                     |
+---------------------------+-----------------------------------------+
| Hardware threads per core | 1                                       |
+---------------------------+-----------------------------------------+
| Hardware threads per node | 128                                     |
+---------------------------+-----------------------------------------+
| Clock rate (GHz)          | ~ 2.45                                  |
+---------------------------+-----------------------------------------+
| RAM (GB)                  | 2,048                                   |
+---------------------------+-----------------------------------------+
| Cache (KB) L1/L2/L3       | 64/512/32768                            |
+---------------------------+-----------------------------------------+
| Local storage (TB)        | 1.5 TB                                  |
+---------------------------+-----------------------------------------+

Login Nodes
--------------
Login nodes provide interactive support for code compilation.

Specialized Nodes
---------------------
Delta will support data transfer nodes (serving the "NCSA Delta" Globus
collection) and nodes in support of other services.

Network
------------
Delta is connected to the NPCF core router & exit infrastructure via two
100Gbps connections, NCSA's 400Gbps+ of WAN connectivity carry traffic
to/from users on an optimal peering.

Delta resources are inter-connected with HPE/Cray's 100Gbps/200Gbps
SlingShot interconnect.

File Systems
---------------

Note:Users of Delta have access to 3 file systems at the time of system
launch, a fourth relaxed-POSIX file system will be made available at a
later date.

**Delta
**\ The Delta storage infrastructure provides users with their HOME and
SCRATCH areas. These file systems are mounted across all Delta nodes and
are accessible on the Delta DTN Endpoints. The aggregate performance of
this subsystem is 70GB/s and it has 6PB of usable space. These file
systems run Lustre via DDN's ExaScaler 6 stack (Lustre 2.14 based).

*Hardware:
*\ DDN SFA7990XE (Quantity: 3), each unit contains

-  One additional SS9012 enclosure
-  168 x 16TB SAS Drives
-  7 x 1.92TB SAS SSDs

The HOME file system has 4 OSTs and is set with a default stripe size of
1.

The SCRATCH file system has 8 OSTs and has Lustre Progressive File
Layout (PFL) enabled which automatically restripes a file as the file
grows. The thresholds for PFL striping for SCRATCH are

========= ============
File size stripe count
0-32M     1 OST
32M-512M  4 OST
512M+     8 OST
========= ============

*Best Practices*

-  To reduce the load on the file system metadata services, the ls
   option for context dependent font coloring, **--**\ color, is
   disabled by default.

*Future Hardware:
* An additional pool of NVME flash from DDN has been installed in early
summer 2022. This flash is initially deployed as a tier for "hot" data
in scratch. This subsystem will have an aggregate performance of 500GB/s
and will have 3PB of raw capacity. As noted above this subsystem will
transition to an independent relaxed POSIX namespace file system,
communications on that timeline will be announced as updates are
available.

Taiga
Taiga is NCSA’s global file system which provides users with their $WORK
area. This file system is mounted across all Delta systems at /taiga
(note that Taiga is used to provision the Delta /projects file system
from /taiga/nsf/delta ) and is accessible on both the Delta and Taiga
DTN endpoints. For NCSA & Illinois researchers, Taiga is also mounted
across NCSA's HAL, HOLL-I, and Radiant compute environments. This
storage subsystem has an aggregate performance of 110GB/s and 1PB of its
capacity allocated to users of the Delta system. /taiga is a Lustre file
system running DDN's Exascaler 6 Lustre stack. See the Taiga and Granite
NCSA wiki site for more information.

*Hardware:
*\ DDN SFA400NVXE (Quantity: 2), each unit contains

-  4 x SS9012 enclosures
-  NVME for metadata and small files

DDN SFA18XE (Quantity: 1), each unit contains

-  10 x SS9012 enclosures
-  NVME for for metadata and small files

$WORK and $SCRATCH

A "module reset" in a job script will populate $WORK and $SCRATCH
environment variables automatically, or you may set them as
WORK=/projects/<account>/$USER , SCRATCH=/scratch/<account>/$USER .

| 

+-------------+-------------+-------------+-------------+-------------+
| **File      | **Quota**   | **          | **Purged**  | **Key       |
| System**    |             | Snapshots** |             | Features**  |
+-------------+-------------+-------------+-------------+-------------+
| HOME (/u)   | **25GB.**   | No/TBA      | No          | Area for    |
|             | 400,000     |             |             | software,   |
|             | files per   |             |             | scripts,    |
|             | user.       |             |             | job files,  |
|             |             |             |             | etc.        |
|             |             |             |             | **NOT**     |
|             |             |             |             | intended as |
|             |             |             |             | a           |
|             |             |             |             | source/     |
|             |             |             |             | destination |
|             |             |             |             | for I/O     |
|             |             |             |             | during jobs |
+-------------+-------------+-------------+-------------+-------------+
| WORK        | **500 GB**. | No/TBA      | No          | Area for    |
| (/projects) | Up to 1-25  |             |             | shared data |
|             | TB by       |             |             | for a       |
|             | allocation  |             |             | project,    |
|             | request.    |             |             | common data |
|             | Large       |             |             | sets,       |
|             | requests    |             |             | software,   |
|             | may have a  |             |             | results,    |
|             | monetary    |             |             | etc.        |
|             | fee.        |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| SCRATCH     | **1000      | No          | Yes**.      | Area for    |
| (/scratch)  | GB**. Up to |             | Purging is  | c           |
|             | 1-100 TB by |             | based on a  | omputation, |
|             | allocation  |             | 30-day last | largest     |
|             | request.    |             | access      | a           |
|             |             |             | policy.     | llocations, |
|             |             |             | \*\*        | where I/O   |
|             |             |             | Purging is  | from jobs   |
|             |             |             | not         | should      |
|             |             |             | currently   | occur       |
|             |             |             | enabled but |             |
|             |             |             | will be     |             |
|             |             |             | when        |             |
|             |             |             | warranted,  |             |
|             |             |             | with a      |             |
|             |             |             | 30-day      |             |
|             |             |             | notice.     |             |
+-------------+-------------+-------------+-------------+-------------+
| /tmp        | **0.74      | No          | After each  | Locally     |
|             | (CPU) or    |             | job         | attached    |
|             | 1.50 TB     |             |             | disk for    |
|             | (GPU)**     |             |             | fast small  |
|             | shared or   |             |             | file IO.    |
|             | dedicated   |             |             |             |
|             | depending   |             |             |             |
|             | on node     |             |             |             |
|             | usage by    |             |             |             |
|             | job(s), no  |             |             |             |
|             | quotas in   |             |             |             |
|             | place       |             |             |             |
+-------------+-------------+-------------+-------------+-------------+

quota usage
           

The **quota** command allows you to view your use of the file systems
and use by your projects. Below is a sample output for a person "user"
who is in two projects: aaaa, and bbbb. The home directory quota does
not depend on which project group the file is written with.

::

   @dt-login01 ~]$ quota
   Quota usage for user :
   -------------------------------------------------------------------------------------------
   | Directory Path | User | User | User  | User | User   | User |
   |                | Block| Soft | Hard  | File | Soft   | Hard |
   |                | Used | Quota| Limit | Used | Quota  | Limit|
   --------------------------------------------------------------------------------------
   | /u/      | 20k  | 25G  | 27.5G | 5    | 300000 | 330000 |
   --------------------------------------------------------------------------------------
   Quota usage for groups user  is a member of:
   -------------------------------------------------------------------------------------
   | Directory Path | Group | Group | Group | Group | Group  | Group |
   |                | Block | Soft  | Hard  | File  | Soft   | Hard  |
   |                | Used  | Quota | Limit | Used  | Quota  | Limit |
   -------------------------------------------------------------------------------------------
   | /projects/aaaa | 8k    | 500G  | 550G  | 2     | 300000 | 330000 |
   | /projects/bbbb | 24k   | 500G  | 550G  | 6     | 300000 | 330000 |
   | /scratch/aaaa  | 8k    | 552G  | 607.2G| 2     | 500000 | 550000 |
   | /scratch/bbbb  | 24k   | 9.766T| 10.74T| 6     | 500000 | 550000 |
   ------------------------------------------------------------------------------------------

File System Dependency Specification for Jobs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We request that jobs specify file system or systems being used in order
for us to respond to resource availability issues. We assume that all
jobs depend on the HOME file system.

Table of Slurm Feature/constraint labels

================= ======================== ==================
File system       Feature/constraint label Note
WORK (/projects)  projects                 
SCRACH (/scratch) scratch                  
IME (/ime)        ime                      depends on scratch
TAIGA (/taiga)    taiga                    
================= ======================== ==================

The Slurm constraint specifier and slurm Feature attribute for jobs are
used to add file system dependencies to a job.

Slurm Feature Specification
^^^^^^^^^^^^^^^^^^^^^^^^^^^

For already submitted and pending (PD) jobs, please use the Slurm
Feature attribute as follows:

::

   $ scontrol update job=JOBID Features="feature1&feature2"]]>
         For already submitted and pending (PD) jobs, please use the Slurm Feature attribute as follows:

   $ scontrol update job=JOBID Features="feature1&feature2"

For example, to add scratch and ime Features to an already submitted
job:

::

   $ scontrol update job=713210 Features="scratch&ime"]]>
         For example, to add scratch and ime Features to an already submitted job:

   $ scontrol update job=713210 Features="scratch&ime"

To verify the setting:

::

   $ scontrol show job 713210 | grep Feature
      Features=scratch&ime DelayBoot=00:00:00

Slurm constraint Specification
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To add Slurm job constraint attributes when submitting a job with sbatch
(or with srun as a command line argument) use the following:

::

   #SBATCH --constraint="constraint1&constraint2.."]]>
         To add Slurm job constraint attributes when submitting a job with sbatch (or with srun as a command line argument) use the following:

   #SBATCH --constraint="constraint1&constraint2.."

For example, to add scratch and ime constraints to when submitting a
job:

::

   #SBATCH --constraint="scratch&ime"]]>
         For example, to add scratch and ime constraints to when submitting a job:

   #SBATCH --constraint="scratch&ime"

To verify the setting:

::

   $ scontrol show job 713267 | grep Feature
      Features=scratch&ime DelayBoot=00:00:00
