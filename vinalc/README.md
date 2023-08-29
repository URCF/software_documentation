---
title: VinaLC
permalink: /VinaLC/
---

**VinaLC**[1][2] is a parallelized version of **Autodock Vina**[3].
VinaLC was created by Lawrence Livermore National Lab.

N.B. VinaLC is not being actively developed. It was last updated in
September 2018.

Picotte
-------

### Installed Versions

**VinalC 1.3.0** is installed on Picotte, compiled both with GCC and
Intel ICC. Use an appropriate modulefile:

`    vinalc/gcc/1.3.0`
`    vinalc/intel/2020/1.3.0`

Please cite the creators of VinaLC appropriately if you use this
software.

### Example Job Script

You will need to compare performance between the two ways of running
VinaLC below.

MPI only:

``` bash
#!/bin/bash
#SBATCH -p def
#SBATCH -t 48:00:00
#SBATCH --nodes=2
#SBATCH --ntasks-per-node=48
#SBATCH --cpus-per-task=1
#SBATCH --mem-per-cpu=3G

module load vinalc/intel/2020/1.3.0

${MPI_RUN} vinalc --recList recList.txt --ligList ligList.txt --geoList geoList.txt
```

Hybrid MPI + threads:

-   --ntasks-per-node -- specifies number of MPI ranks (processes)
-   --cpus-per-task -- specifies number of threads per rank; we want one
    CPU core per thread

``` bash
#!/bin/bash
#SBATCH -p def
#SBATCH -t 48:00:00
#SBATCH --nodes=2
#SBATCH --ntasks-per-node=6
#SBATCH --cpus-per-task=8
#SBATCH --mem-per-cpu=3G

module load vinalc/intel/2020/1.3.0

export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
export OMP_PROC_BIND=true
${MPI_RUN} vinalc --recList recList.txt --ligList ligList.txt --geoList geoList.txt
```

Proteus
-------

### Installed Version

**VinaLC 1.1.2** is installed on Proteus. Use the following modulefile:

`   vinalc/intel/2015/1.1.2`

It is compiled with Intel Composer XE, and Open MPI 1.8.1. For
information on running an Open MPI program, see [Message Passing Interface\#OpenMPI](/Message_Passing_Interface#OpenMPI "wikilink")

This will run only on Intel nodes.

Please cite the creators of VinaLC appropriately if you use this
software.

### Example Job Script

This example job script uses the testcase distributed with the VinaLC
source code.

N.B. There are two executables: `vinaBMPI`, and `vinalc`. Both seem to
run correctly on the test case, but the documentation does not make
clear the difference between the two. Non-rigorous tests show that the
`vinalc` program runs slightly faster. It may benefit from hybrid
threading-MPI execution. See: [Hybrid MPI-OpenMP Jobs\#OpenMPI - Usage](/Hybrid_MPI-OpenMP_Jobs#OpenMPI_-_Usage "wikilink")

``` bash
#!/bin/bash
#$ -S /bin/bash
#$ -cwd
#$ -j y
#$ -P FIXME
#$ -M FIXME
#$ -pe fixed16 64
#$ -l ua=sandybridge
#$ -l h_rt=1:00:00
#$ -l m_mem_free=3g
#$ -l h_vmem=4G
. /etc/profile.d/modules.sh
module load shared
module load proteus
module load sge/univa
module load gcc/4.8.1
module load vinalc/intel/2015/1.1.2

# echo "Starting vinaBMPI"
# ${MPI_RUN} vinaBMPI --recList recList.txt --ligList ligList.txt --geoList geoList.txt
# echo "Done vinaBMPI"

echo "Starting vinalc"
${MPI_RUN} vinalc --recList recList.txt --ligList ligList.txt --geoList geoList.txt
echo "Done vinalc"
```

See Also
--------

-   [Compiling VinaLC](/Compiling_VinaLC "wikilink")

References
----------

<references/>

[1] [VinaLC official website](https://plsuser.llnl.gov/bbs/vinalc/index.html)

[2] Xiaohua Zhang, Sergio E. Wong, and Felice C. Lightstone. (2013)
Message Passing Interface and Multithreading Hybrid for Parallel
Molecular Docking of Large Databases on Petascale High Performance
Computing Machines. J. Comput. Chem.
[<DOI:10.1002/jcc.23214>](https://doi.org/10.1002/jcc.23214)

[3] [Autodock Vina official website](http://vina.scripps.edu/)