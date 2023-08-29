---
title: LAMMPS
permalink: /LAMMPS/
---

**LAMMPS** is a classical molecular dynamics code, and an acronym for
Large-scale Atomic/Molecular Massively Parallel Simulator.[1]

Picotte
=======

Currently, LAMMPS is not globally installed on Picotte.

Installing Your Own
-------------------

A pre-compiled version is available on Conda Forge. Install with
miniconda: <https://anaconda.org/conda-forge/lammps>

Proteus (OBSOLETE)
==================

Installed Version(s)
--------------------

Currently, LAMMPS is installed only on Proteus. The following versions
are installed:

-   LAMMPS stable_11Aug2017 (incl. updates up to 17-Jan-2018)
-   LAMMPS stable_16Mar2018 using GPU
-   LAMMPS stable_5Jun2019

Use one of the modulefile(s):

`    lammps/intel/2015/11Aug2017 -- This is an `**`Intel-only`**` build.  This is built with `**`Python`` ``3`**` integration.`
`    lammps/16Mar2018            -- Available once the `**`proteus-gpu`**` modulefile is loaded. This is a `**`CUDA-enabled`**` build. This uses `**`Python`` ``3`**` integration.`
`    lammps/intel/2019/5Jun2019  -- This is for the new `**`Skylake`**` nodes (ic23) only in new.q. Recommended NOT to use OMP. `
`                                   Preliminary benchmarks show up to 3x performance when NOT using hybrid OMP-MPI.`

### On new.q

On the new nodes in new.q:

-   lammps/gcc/5Jun2019
-   lammps/intel/2019/5Jun2019

Running
-------

With the hybrid MPI-OpenMP code, you have many options for distributing
the compute processes over many nodes, plus options for core binding
(aka processor affinity, aka CPU pinning). Like anything else to do with
computational performance, your mileage may vary. You have to benchmark
your code to see which "optimizations" actually lead to performance
improvements.

### Running Benchmarks

We will run the **in.lj** benchmark. The examples here use two nodes,
and all the slots on each node.

Create the following job script in `$LAMMPS_SRC/bench/` and name it
`lammpsbench.sh`:

``` bash
#!/bin/bash
#$ -S /bin/bash
#$ -M juser@drexel.edu
#$ -P myrsrchPrj
#$ -cwd
#$ -j y
#$ -q all.q
#$ -R y
#$ -pe fixed16 32
#$ -l h_rt=600
#$ -l m_mem_free=2g
#$ -l h_vmem=4g
#$ -l vendor=intel

. /etc/profile.d/modules.sh
module load shared
module load proteus
module load gcc
module load sge/univa
module load lammps/intel/2015/11Aug2017

### no. of threads per MPI process (rank)
### NOTE: add this line to your LAMMPS input file -- see http://lammps.sandia.gov/doc/processors.html
###
###       processors * * * grid numa
export OMP_NUM_THREADS=16  ### FIXME - do some benchmarks to find an appropriate value

### The case OMP_NUM_THREADS=1 is equivalent to MPI without threads

### input file
export INPUT=in.lj

### log file (instead of log.lammps)
export LOGFILE=log.proteus.$(echo ${INPUT} | cut -f2 -d.).${NSLOTS}.${NDIV}

### or simpler version
## export LOGFILE=log.lammps.${JOB_ID}

### LAMMPS options for OMP threads
export LMPOPTS="-sf omp -pk omp ${OMP_NUM_THREADS}"

### mpirun options to export necessary environment variables
export MPIRUNOPTS="-x LD_LIBRARY_PATH -x OMP_NUM_THREADS"

### if you are running a copy of LAMMPS that you compiled yourself, replace "${LAMMPS_EXE}" with the full path to your LAMMPS executable
${MPI_RUN} --map-by node:PE=${OMP_NUM_THREADS} ${MPIRUNOPTS} ${LAMMPS_EXE} ${LMPOPTS} -in ${INPUT} -log ${LOGFILE}
```

This takes only a few seconds to run. The output will be in the file
`log.lammps` as well as the normal job output file,
`lammpsbench.sh.oNNNNNN`. Editing out the included input file, the
output should look like:

``` text
LAMMPS (14 May 2016)
  using 16 OpenMP thread(s) per MPI task
package omp 0
using multi-threaded neighbor list subroutines
package omp 16
using multi-threaded neighbor list subroutines
...
Last active /omp style is pair_style lj/cut/omp
Neighbor list info ...
  1 neighbor list requests
  update every 20 steps, delay 0 steps, check no
  max neighbors/atom: 2000, page size: 100000
  master list distance cutoff = 2.8
  ghost atom cutoff = 2.8
  binsize = 1.4 -> bins = 24 24 24
Memory usage per processor = 19.4975 Mbytes
Step Temp E_pair E_mol TotEng Press
       0         1.44   -6.7733681            0   -4.6134356   -5.0197073
     100    0.7574531   -5.7585055            0   -4.6223613   0.20726105
Loop time of 0.153996 on 32 procs for 100 steps with 32000 atoms

Performance: 280526.784 tau/day, 649.368 timesteps/s
1594.6% CPU use with 2 MPI tasks x 16 OpenMP threads

MPI task timing breakdown:
Section |  min time  |  avg time  |  max time  |%varavg| %total
---------------------------------------------------------------
Pair    | 0.074429   | 0.074459   | 0.074489   |   0.0 | 48.35
Neigh   | 0.010757   | 0.014999   | 0.019241   |   3.5 |  9.74
Comm    | 0.038196   | 0.044272   | 0.050349   |   2.9 | 28.75
Output  | 8.7976e-05 | 9.8467e-05 | 0.00010896 |   0.1 |  0.06
Modify  | 0.018245   | 0.020058   | 0.02187    |   1.3 | 13.02
Other   |            | 0.0001096  |            |       |  0.07

Nlocal:    16000 ave 16001 max 15999 min
Histogram: 1 0 0 0 0 0 0 0 0 1
Nghost:    13632.5 ave 13635 max 13630 min
Histogram: 1 0 0 0 0 0 0 0 0 1
Neighs:    601416 ave 605200 max 597633 min
Histogram: 1 0 0 0 0 0 0 0 0 1

Total # of neighbors = 1202833
Ave neighs/atom = 37.5885
Neighbor list builds = 5
Dangerous builds not checked
Total wall time: 0:00:00
```

Note that this job did run on two nodes with two MPI tasks and 16
threads per task, as wanted:

``` text
Performance: 280526.784 tau/day, 649.368 timesteps/s
1594.6% CPU use with 2 MPI tasks x 16 OpenMP threads
```

### Benchmark Results with Different Slot Distributions

These benchmarks were performed on the standard Proteus Intel nodes,
which are 16-core Intel Sandy Bridge machines.

This is a summary of the results of running the **`in.lj`** benchmark,
modified to run 10,000 timesteps, with the Intel-compiled **LAMMPS
16Feb16**, using different numbers of slots, and different divisions of
processors. All were run using the `fixed16` PE, which assigns
"complete" Intel nodes, ensuring that no other jobs are sharing the same
resources.

Runs were done with "-pe shm 16", "-pe fixed16 32", "-pe fixed16 48",
and "-pe fixed16 64". The variable `OMP_NUM_THREADS` in the job script
was varied to distribute the MPI tasks and OMP threads per task
differently.

The best performance seems to come with running 4 threads per MPI task.
OMP_NUM_THREADS is the number of threads per MPI rank.

**IMPORTANT NOTE** Performance strongly depends on the type of problem.
This benchmark problem may not resemble the problems you are solving,
and hence this benchmark may not offer good guidance for how your runs
should be performed.

<table>
<thead>
<tr class="header">
<th><p>NSLOTS</p></th>
<th><p>OMP_NUM_THREADS</p></th>
<th><p>Performance<br />
(timesteps/sec)</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>16</p></td>
<td><p>16</p></td>
<td><p>415.089</p></td>
</tr>
<tr class="even">
<td><p>8</p></td>
<td><p>599.786</p></td>
</tr>
<tr class="odd">
<td><p>4</p></td>
<td><p>627.102</p></td>
</tr>
<tr class="even">
<td><p>1</p></td>
<td><p>712.838</p></td>
</tr>
<tr class="odd">
<td><p>32</p></td>
<td><p>16</p></td>
<td><p>609.115</p></td>
</tr>
<tr class="even">
<td><p>8</p></td>
<td><p>828.216</p></td>
</tr>
<tr class="odd">
<td><p>4</p></td>
<td><p>861.781</p></td>
</tr>
<tr class="even">
<td><p>1</p></td>
<td><p>747.488</p></td>
</tr>
<tr class="odd">
<td><p>48</p></td>
<td><p>16</p></td>
<td><p>787.374</p></td>
</tr>
<tr class="even">
<td><p>8</p></td>
<td><p>1047.598</p></td>
</tr>
<tr class="odd">
<td><p>4</p></td>
<td><p>1055.877</p></td>
</tr>
<tr class="even">
<td><p>1</p></td>
<td><p>959.972</p></td>
</tr>
<tr class="odd">
<td><p>64</p></td>
<td><p>16</p></td>
<td><p>801.444</p></td>
</tr>
<tr class="even">
<td><p>8</p></td>
<td><p>1119.477</p></td>
</tr>
<tr class="odd">
<td><p>4</p></td>
<td><p>1224.945</p></td>
</tr>
<tr class="even">
<td><p>1</p></td>
<td><p>1270.930</p></td>
</tr>
<tr class="odd">
<td><p>256</p></td>
<td><p>16</p></td>
<td><p>1668.267</p></td>
</tr>
<tr class="even">
<td><p>8</p></td>
<td><p>2086.822</p></td>
</tr>
<tr class="odd">
<td><p>4</p></td>
<td><p>2542.448</p></td>
</tr>
<tr class="even">
<td><p>1</p></td>
<td><p>1490.833</p></td>
</tr>
</tbody>
</table>

![900px](/Lammps_performance.svg "wikilink")

### Core-Binding and NUMA

This is optional. It may improve performance by preventing compute
threads from being assigned to different processor cores over the
lifetime of the computation. This reduces the overhead involved in
migrating a computation from one core to another.

Grid Engine produces a host file which OpenMPI can parse to read the
binding information. See the Univa Grid Engine User Guide.[2][3] There
is also information in the qsub man page.

### CUDA-enabled Version

See official documentation for details, including recommendations for
running: <https://lammps.sandia.gov/doc/accelerate_gpu.html>

**NOTES**

-   LAMMPS official documentation recommends oversubscribing use of
    GPUs, i.e. using more than one MPI rank per GPU device. (This is
    contrary to typical advice to use only one MPI rank per GPU device
    as a "feeder" process.)
    -   The obvious setting of 2 MPI ranks, one rank for each GPU
        device, results in a fairly low GPU utilization, about 35%.
    -   Oversubscribing, i.e. running more than 2 MPI ranks, increases
        GPU utilization. Some benchmarking for your specific application
        needs to be done in order to figure out an optimum
        oversubscription level. See:
        <https://lammps.sandia.gov/doc/accelerate_gpu.html>
    -   The GPU utilization does not scale linearly with
        oversubscription. You can view the GPU utilization via Ganglia,
        e.g. [this shows the utilization of GPU device \#0 (`gpu0_util`)](https://proteusmaster.urcf.drexel.edu/ganglia-proteus/?r=hour&cs=&ce=&c=Proteus&h=&tab=m&vn=&hide-hf=false&m=gpu0_util&sh=1&z=small&hc=4&host_regex=&max_graphs=0&s=by+name).
    -   For some reason, requesting anything other than the "fixed16"
        PE, i.e. fewer slots per node than the total available, results
        in a failed job.
    -   You have to then manually specify the total number of ranks to
        be run ("-np"), and the number of ranks per node ("-npernode")

The CUDA-enabled version uses different options.

-   Please see the official LAMMPS documentation for more details on
    using GPU acceleration.[4]
-   There may be modifications which have to be made to the input files
    to use GPUs.
-   LAMMPS authors have published benchmarks comparing the various
    acceleration implementations.[5]
-   Use the "`-sf gpu`" command line switch to enable use of GPUs
-   Use the "`-pk gpu `*`N`*" command line to specify the number of GPU
    devices per node to use (up to 2)
-   ~~Number of MPI ranks, OMP threads, etc are set according to NVIDIA
    recommendations.[6] See the [NAMD](/NAMD "wikilink") article for
    details.~~
-   The USER-OMP extension which provides multithreaded execution using
    OpenMP seems to be mutually exclusive with the GPU extension. So,
    the NVIDIA recommendation to run hybrid MPI/OpenMP does not seem to
    help.

Example:

``` bash
#!/bin/bash -l
#$ -S /bin/bash
#$ -cwd
#$ -M FIXME@drexel.edu
#$ -P FIXME
#$ -j y
### IMPORTANT can only request fixed16
#$ -pe fixed16 32
#$ -l gpu=2
#$ -l h_rt=12:00:00
#$ -l h_vmem=200G
#$ -q gpu.q

module load shared
module load proteus
module load proteus-gpu
module load gcc
module load sge/univa
module load lammps/16Mar2018

module list


export INFILE=./in.lj

### LAMMPS options to use GPU, 2 GPU devices per node.
### CANNOT use the omp extension together with GPU
export LMOPTS="-sf gpu -pk gpu 2"

### want to run 8 ranks per node, i.e. 16 ranks total
### even though we request 32 slots total (16 slots per node)
export MPIOPTS="-np 16 -npernode 8"

echo "Starting $LAMMPS_EXE in $( pwd ) ..."

${MPI_RUN} ${MPIOPTS} ${LAMMPS_EXE} ${LMOPTS} -in ${INFILE} -log log.lammps.${JOB_ID}
```

In the output, you should see a message listing the GPU devices being
used:

``` text
- Using acceleration for lj/cut:
-  with 1 proc(s) per device.
-  with 1 thread(s) per proc.
--------------------------------------------------------------------------
Device 0: Tesla K20Xm, 14 CUs, 5.5/5.6 GB, 0.73 GHZ (Mixed Precision)
Device 1: Tesla K20Xm, 14 CUs, 0.73 GHZ (Mixed Precision)
--------------------------------------------------------------------------

Initializing Device and compiling on process 0...Done.
Initializing Devices 0-1 on core 0...Done.
```

See also: [GPU Jobs](/GPU_Jobs "wikilink")

Compiling
---------

-   See [Compiling LAMMPS](/Compiling_LAMMPS "wikilink")

References
----------

<references/>

[1] [LAMMPS official web site](http://lammps.sandia.gov/)

[2] [Univa Grid Engine User Guide - §7.4 Jobs with Core Binding](https://proteusmaster.urcf.drexel.edu/UGE/user_guide.html#Jobs_with_Core_Binding_.5Bupdate_8.1.5D)

[3] [Univa Grid Engine User Guide - §7.5 NUMA Aware Jobs: Jobs with Memory Binding and Enhanced Memory Management](https://proteusmaster.urcf.drexel.edu/UGE/user_guide.html#NUMA_Aware_Jobs:_Jobs_with_Memory_Binding_and_Enhanced_Memory_Management_.5Bsince_8.1.5D)

[4] [LAMMPS Documentation: Accelerating LAMMPS Performance: GPU package](http://lammps.sandia.gov/doc/accelerate_gpu.html)

[5] [LAMMPS Benchmarks: Oct 2016, CPU vs GPU vs KNL performance](https://lammps.sandia.gov/bench.html#accelerator)

[6] [NVIDIA Hardware & Software Cofigurations: GPU-accelerated NAMD](https://www.nvidia.com/en-us/data-center/gpu-accelerated-applications/namd/) (see the "Command Line Options" section)