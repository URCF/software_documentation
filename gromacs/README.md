---
title: GROMACS
permalink: /GROMACS/
---

**GROMACS** is a molecular dynamics software package.[1]

Picotte
=======

All installed versions are **single precision**.

Installed Versions
------------------

**GROMACS 2021.1**, **2021.3**, and **2023** are installed.

### CPU-only versions

This MPI-enabled, compiled with Intel compilers and using Intel MKL for
FFTW3. Use the appropriate modulefile:

`   gromacs/intel/2020/2021.1`
`   gromacs/intel/2020/2021.3`

### CUDA (GPU) versions

There are two versions installed.

**2021.1** is non-MPI, i.e. single node only. It uses GROMACS's "thread
MPI" and OpenMP in order to use multiple GPU devices.

`   gromacs/cuda11.0/2021.1`

**2021.3** is MPI, i.e. multi-node. It is also compiled with Intel
compilers.

`   gromacs/cuda11.2/2021.3`

**2023** is MPI, i.e. multi-node. It is compiled with GCC.

`   gromacs/cuda11.4/2023`

Example Job Scripts
-------------------

### CPU-only version

``` bash
#!/bin/bash
#SBATCH --nodes=4
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=6
#SBATCH --mem=180G
#SBATCH --time=12:00:00

module load gromacs/intel/2020/2021.3

### consult Gromacs documentation for appropriate command to use to run MPI

$MPI_RUN $GMXBIN/gmx_mpi mdrun -s something.tpr ...
```

### CUDA version

``` bash
#!/bin/bash
#SBATCH -p gpu
#SBATCH --nodes=3
#SBATCH --ntasks-per-node=4
#SBATCH --cpus-per-task=12
#SBATCH --gres=gpu:4
#SBATCH --gpu-bind=closest
#SBATCH --gres-flags=enforce-binding
#SBATCH --mem-per-gpu=40G
#SBATCH --time=12:00:00

### Modulefiles for software using CUDA
module use /ifs/opt_cuda/modulefiles
module load gromacs/cuda11.2/2021.3

### NOTES
### * each GPU is assigned its own MPI rank (ntasks-per-node)
### * each MPI rank runs multithreaded (cpus-per-task)

export NRANKS=$( expr $SLURM_JOB_NUM_NODES \* $SLURM_TASKS_PER_NODE )

### number of threads per rank is specified by OMP_NUM_THREADS
### using any value > 6 will trigger a warning from GROMACS

### benchmarking indicates 5 threads per rank produces best performance
### YMMV please benchmark performance on your own simulations
export OMP_NUM_THREADS=5

### consult Gromacs documentation for appropriate command to use to run MPI+GPU

$MPI_RUN -n $NRANKS $GMXBIN/gmx_mpi mdrun -s something.tpr -ntomp $OMP_NUM_THREADS -nb gpu -gpu_id 0123
```

Benchmarking
------------

-   Automated benchmark generation:
    [MDBenchmark](https://mdbenchmark.readthedocs.io/en/latest/)
-   Benchmark inputs from Max Planck Inst. for Biophysical Chemistry:
    <https://www.mpibpc.mpg.de/grubmueller/bench>
    -   Updated shell scripts and a Slurm job script:
        <https://github.com/prehensilecode/mpibpc-gromacs-bench-utils>

Installing Your Own
-------------------

Bioconda provides pre-compiled versions that can be installed using
Miniconda:

-   <https://anaconda.org/bioconda/gromacs> (non-MPI, i.e. single node)
-   <https://anaconda.org/bioconda/gromacs_mpi> (MPI, i.e. multiple
    node)

Proteus (OBSOLETE)
==================

Installed Versions
------------------

**GROMACS 2016.2**, **2016.4**, and **2018.1** compiled with Intel
Composer XE 2015.1.133 + MKL and OpenMPI are installed on Proteus.
Select the modulefile to load:

`   gromacs/intel/2015/2016.2`
`   gromacs/intel/2015/2016.4`
`   gromacs/intel/2015/2018.1`

**GROMACS 2018.2** compiled with CUDA is also installed. It is provided
after the `proteus-gpu` modulefile is loaded. It will be in
`/mnt/HA/opt_cuda90/modulefiles`:

`   gromacs/2018.2`

All are compiled with default precision, i.e. **single**.

For the new 40-core Skylake-based nodes, in the queue **`new.q`**:

`   gromacs/intel/2019/2019.2`

For compilation instructions, see: [Compiling GROMACS](/Compiling_GROMACS "wikilink")

Running
-------

See the official documentation on obtaining good performance from
mdrun.[2]

### CPU version

Note that the GROMACS program executable is **gmx_mpi**. An alias
**gmx** is defined.

Example script (common portions missing - please fill in for yourself):

``` bash
#$ -pe fixed16 64
#$ -l ua=sandybridge
#$ -l h_vmem=3000M
#$ -l m_mem_free=2G
#$ -l h_rt=2:00:00

. /etc/profile.d/modules.sh
module load shared
module load proteus
module load gcc/4.8.1
module load sge/univa
module load gromacs/intel/2015/2016.2

### It is generally more efficient to run fewer MPI ranks (processes),
### with each rank running some number of OpenMP threads.
### The following runs 16 ranks with 4 threads each; note that the
### total number of threads must be equal to the number of slots
### requested: 16 * 4 = 64
### This also assumes that each host (node) has 16 CPU cores.

# the "expr" program does arithmetic
export OMP_NUM_THREADS=4
export NRANKS=$( expr ${NHOSTS} \* 4 )
export MPIOPTS="-x OMP_NUM_THREADS -np ${NRANKS} --map-by node:PE=${OMP_NUM_THREADS}"

### DEPRECATED phase out use of this next line
# ${MPI_RUN} -np ${NRANKS} ${GMXBIN}/gmx_mpi mdrun -ntomp ${NUM_THREADS} ...

### Use the following line
${MPI_RUN} ${MPIOPTS} ${GMXBIN}/gmx_mpi mdrun -deffnm ethanol.0 -dhdl ethanol.0.dhdl.xvg
```

For more details on running, see the articles on [MPI](/MPI "wikilink")
and [Writing Job Scripts](/Writing_Job_Scripts "wikilink")

For a similar application, see how performance of LAMMPS changes as the
distribution of MPI ranks and OpenMP threads changes:
[LAMMPS\#Benchmark_Results_with_Different_Slot_Distributions](/LAMMPS#Benchmark_Results_with_Different_Slot_Distributions "wikilink")

In the GROMACS log, you should see messages about the hardware, and the
number of ranks and threads:

``` text
Running on 2 nodes with total 32 cores, 32 logical cores
  Cores per node:           16
  Logical cores per node:   16
...
Using 8 MPI processes
Using 4 OpenMP threads per MPI process
```

### New CPU Version for Haswell nodes in `new.q`

T.B.A.

### CUDA-enabled Version

GROMACS has GPU acceleration built in.[3] It is a heterogeneous
parallelization, using both MPI and CUDA.[4]

Note that no separate openmpi modulefile should be loaded. This build of
GROMACS uses the Open MPI that is provided by [Mellanox HPC-X](http://www.mellanox.com/page/hpcx_overview).

To enable use of the GPU devices, you must set:

`   cutoff-scheme = Verlet`

GROMACS also suggests changes in the values of `nstlist` and `rlist`.
(See below.) This means that you should not just re-use the input files
from a CPU-only run.

NVIDIA has specific recommendations for distributing ranks and threads
for multi-node NAMD jobs on GPUs. (See the summary at
[NAMD\#CUDA-enabled Version using IBVERBS](/NAMD#CUDA-enabled_Version_using_IBVERBS "wikilink").) The
recommendations may apply to GROMACS, as well.

Here is an example job script using the NVIDIA recommendations:

``` bash
#!/bin/bash -l
#$ -S /bin/bash
#$ -cwd
#$ -j y
#$ -M FIXME
#$ -P FIXME
#$ -pe fixed16 32
#$ -q gpu.q
#$ -l excl
#$ -l gpu=2
#$ -l h_rt=24:00:00
#$ -l m_mem_free=3G
#$ -l h_vmem=200G

module load gromacs/2018.2

### the following values are based on Nvidia's recommendations for NAMD
# there are 2 devices per host; we want 1 MPI rank per device to feed work to the devices
# we want the non-GPU work to be multithreaded:
#    we want the total number of threads (NPES) = total no. of slots - total no. of GPU devices
export NPES=$( expr ${NSLOTS} - ${NHOSTS} \* 2 )

# number of OMP threads per MPI rank is ((no. of CPU cores per host / no. of GPU devices per host) - 1)
export OMP_NUM_THREADS=$( expr 16 / 2 - 1 )

export NRANKS=$( expr ${NPES} / ${OMP_NUM_THREADS} )

### so, if we have 32 slots on 2 hosts,
###     NPES = 28
###     OMP_NUM_THREADS = 7
###     NRANKS = 4
### i.e. each host will have 2 ranks (i.e. one rank per GPU device), and each rank
###      runs 7 threads => 28 threads total; the 4 "leftover" CPU cores are
###      free to deal with communicating with the GPU devices

export MPIOPTS="-x OMP_NUM_THREADS -np ${NRANKS} --map-by node:PE=${OMP_NUM_THREADS}"

echo "OMP_NUM_THREADS = ${OMP_NUM_THREADS}"
echo "NRANKS = ${NRANKS}"
echo "MPIOPTS = ${MPIOPTS}"

${MPI_RUN} ${MPIOPTS} ${GMXBIN}/gmx_mpi mdrun -deffnm ethanol.0 -dhdl ethanol.0.dhdl.xvg
```

Note that:

-   the PE is **fixed16**

In the GROMACS log, you should see a message listing the GPU devices to
be used:

``` text

Running on 2 nodes with total 32 cores, 32 logical cores, 4 compatible GPUs
  Cores per node:           16
  Logical cores per node:   16
  Compatible GPUs per node:  2
  All nodes have identical type(s) of GPUs
Hardware detected on host gpu02 (the node of MPI rank 0):
  CPU info:
    Vendor: Intel
    Brand:  Intel(R) Xeon(R) CPU E5-2650 v2 @ 2.60GHz
    Family: 6   Model: 62   Stepping: 4
...
  GPU info:
    Number of GPUs detected: 2
    #0: NVIDIA Tesla K20Xm, compute cap.: 3.5, ECC: yes, stat: compatible
    #1: NVIDIA Tesla K20Xm, compute cap.: 3.5, ECC: yes, stat: compatible
...
The number of OpenMP threads was set by environment variable OMP_NUM_THREADS to 8
Using 4 MPI processes
Using 8 OpenMP threads per MPI process
```

Note that it also advises some changes in parameters:

``` text
For optimal performance with a GPU nstlist (now 10) should be larger.
The optimum depends on your CPU and GPU resources.
You might want to try several nstlist values.
Changing nstlist from 10 to 25, rlist from 0.932 to 1.022
```

See also: [GPU Jobs](/GPU_Jobs "wikilink") -- which will explain why the
`h_vmem` value is so high.

Benchmarks
----------

-   GROMACS authors have a benchmark suite: [GROMACS Benchmarks](http://www.gromacs.org/About_Gromacs/Benchmarks)
-   The HPC Advisory Council has published some benchmarks for a
    specific Intel Haswell CPU with InfiniBand FDR & EDR:
    [<File:GROMACS> Analysis Intel E5 2697v3.pdf](/GROMACS_Analysis_Intel_E5_2697v3.pdf "wikilink")

### Comparison Between CPU-only Version with CUDA-enabled Version

A non-rigorous benchmark showed these performance metrics:

| GROMACS flavor                                               | Performance in ns/day | Performance in day/ns | Performance in hour/ns |
|--------------------------------------------------------------|-----------------------|-----------------------|------------------------|
| CPU-only 32 slots Sandy Bridge (using Intel compilers + MKL) | 230.719               | 0.00433               | 0.104                  |
| CUDA-enabled (4 GPU devices)                                 | 259.479               | 0.00385               | 0.092                  |

See Also
--------

-   [Compiling GROMACS](/Compiling_GROMACS "wikilink")

References
----------

<references/>

[1] [Gromacs website](http://www.gromacs.org/)

[2] [GROMACS 2016.5 User Guide: Getting good performance from mdrun](http://manual.gromacs.org/documentation/2016.5/user-guide/mdrun-performance.html)

[3] [GROMACS Documentation: GPU acceleration](http://www.gromacs.org/GPU_acceleration)

[4] [GROMACS Documentation: Acceleration and parallelization: Heterogeneous parallelization](http://www.gromacs.org/Documentation/Acceleration_and_parallelization#Heterogenous_parallelization.3a_using_GPUs)
