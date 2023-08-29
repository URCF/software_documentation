---
title: STAR-CCM+
permalink: /STAR-CCM+/
---

**STAR-CCM+** is an engineering simulation package from CD-Adapco.[1]
The installation also includes STAR-View

Installed Versions
------------------

Proteus provides:

-   STAR-CCM+ 11.04.012-R8 with STAR-View-11.04.012
-   STAR-CCM+ 10.06.009 with STAR-View 10.06.009
-   STAR-CCM+ 9.06.011-R8 with STAR-View 9.06.011

These are provided by the
[modulefiles](/Environment_Modules_Quick_Start_Guide "wikilink"):

`   cd-adapco/starccm+/11.04.012-r8`
`   cd-adapco/starccm+/10.06.009`
`   cd-adapco/starccm+/9.06.011-r8`

### License Status

License information may be viewed at:

`    `[`https://proteusmaster.urcf.drexel.edu/lmstat.html`](https://proteusmaster.urcf.drexel.edu/lmstat.html)

Documentation
-------------

Documentation is provided as PDF file, in the location:

`   ${STARCCMDIR}/doc/UserGuide_MM.NN.pdf`

where "MM.NN" should be replaced by the appropriate version number.

Integration with Grid Engine
----------------------------

STAR-CCM+ has Grid Engine integration built in. See the section "Using
Sun Grid Engine" on p.398 of the User Guide.

### Running a Batch Job on Proteus

-   Use the parallel environment named "starccm" with no more than 16
    slots (STAR-CCM+ is licensed only for 16 cores, so no more than 16
    slots may be requested)
-   Use the fabric "IBV" (InfiniBand using OFED Verbs)
-   If you copy your job to the [Lustre Scratch Filesystem](/Lustre_Scratch_Filesystem "wikilink"), you can use
    "Parallel I/O" by specifying "-pio"

`    starccm+ -batch -batchsystem sge -fabricverbose -fabric IBV -cpubind something.sim`

### Example Job Script

-   Create a job script named "myjob.sh" -- follow the example below[2]
-   Submit the job with: qsub myjob.sh

``` bash
#!/bin/bash
#$ -S /bin/bash
#$ -P myresearchPrj
#$ -cwd
#$ -j y
#$ -M FIXME@drexel.edu
#$ -pe starccm 16
#$ -l h_rt=24:00:00
#$ -l h_vmem=4g
#$ -l m_mem_free=2g
#$ -l vendor=intel
. /etc/profile.d/modules.sh
module load shared
module load proteus
module load gcc/4.8.1
module load cd-adapco/starccm+/10.06.009

starccm+ -batch -batchsystem sge -fabricverbose -fabric IBV -cpubind SIM_TEST.sim
sleep 120
```

In the output file, which should be named something like
`myjob.sh.o277823`, you should see something like the following:

``` text
Starting local server: /mnt/HA/opt/CD-adapco/star-ccm+/10.06.009/STAR-CCM+10.06.009/star/bin/starccm+ -batchsystem sge -fabricverbose -fabric IBV -cpubind -server SIM_TEST.sim
Info: Processes will be placed on CPU/Cores in a round robin fashion.
Starting STAR-CCM+ parallel server
Host 0 -- ip 172.16.1.54 -- ranks 0 - 15

 host | 0
======|======
    0 : SHM

 Prot -  All Intra-node communication is: SHM

MPI Distribution : Platform Computing MPI-09.01.03.01
Host 0 -- ic13n03.cm.cluster -- Ranks 0-15
Process rank 0 ic13n03.cm.cluster 89635
Total number of processes : 16

STAR-CCM+ 10.06.009 (linux-x86_64-2.5/gnu4.8)
License build date: 10 February 2015
This version of the code requires license version 2015.10 or greater.
Checking license file: /cm/shared/licenses/lmgrd/license.dat
Checking license file: 28518@proteusmaster.cm.cluster
1 copy of ccmpsuite checked out from /cm/shared/licenses/lmgrd/license.dat
Feature ccmpsuite expires in 9 days
Wed Dec  2 11:50:27 2015

Server::start -host ic13n03.cm.cluster:47827
Server idle time limit is 1 hours.

Loading simulation database (w/ parallel I/O): SIM_TEST.sim
Loading module: CadModeler
Started Parasolid modeler version 28.00.151
Loading module: MeshingSurfaceRepair
Loading module: StarResurfacer
Loading module: StarDualMesher
Loading module: CoupledFlowModel
Loading module: SegregatedFlowModel
Loading module: KeTurbModel
Simulation database saved by:
  STAR-CCM+ 9.06.011 (win64/intel12.1-r8) Mon Nov 10 22:24:21 UTC 2014 Serial
Loading into:
  STAR-CCM+ 10.06.009 (linux-x86_64-2.5/gnu4.8) Tue Oct 6 20:33:26 UTC 2015 Np=16
Simulation database load completed.
Running
15 copies of hpcdomains checked out from /cm/shared/licenses/lmgrd/license.dat
Feature hpcdomains expires in 9 days
Loading/configuring connectivity (old|new partitions: 1|16)
  Body 1 (index 0): 180884 cells, 1240002 faces, 1069661 verts.
Configuring finished
Reading material property database "/mnt/HA/opt/CD-adapco/star-ccm+/10.06.009/STAR-CCM+10.06.009/star/props.mdb"...
Re-partitioning
Reversed flow on  112 faces on Outlet
     Iteration     Continuity     X-momentum     Y-momentum     Z-momentum            Tke            Tdr
             1   1.000000e+00   1.000000e+00   1.000000e+00   1.000000e+00   1.000000e+00   1.000000e+00
Reversed flow on  36 faces on Outlet
             2   4.636743e-01   1.000000e+00   1.000000e+00   1.000000e+00   7.932383e-01   7.720807e-01
Reversed flow on  97 faces on Outlet
             3   2.504759e-01   5.455403e-01   5.879972e-01   4.662518e-01   6.365401e-01   6.126451e-01
...
Stopping criterion Maximum Steps satisfied.
Auto save: SIM_TEST@01000.sim
Saving (w/ parallel I/O): SIM_TEST@01000.sim
Simulation saved to SIM_TEST@01000.sim (84.221MB in 0.62764s).
Design STAR-CCM+ simulation completed
Server process exited with code 0
```

References
----------

<references/>

[1] [STAR-CCM+ product info at CD-Adapco](http://www.cd-adapco.com/products/star-ccm%C2%AE)

[2] [Writing Job Scripts](/Writing_Job_Scripts "wikilink")