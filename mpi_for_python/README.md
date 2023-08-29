---
title: MPI for Python
permalink: /MPI_for_Python/
---

MPI for Python (aka mpi4py) is a Python binding for
[MPI](/MPI "wikilink").[1]

Installing
----------

None of the module-provided Pythons have mpi4py pre-installed. This is
because there is a choice of [MPI](/MPI "wikilink") implementation that
needs to be made when building the mpi4py package. You should install
your own version, using pip. Note that you will need an MPI module
loaded, first.

`    [juser@picotte001 ~]$ module load python/gcc`
`    [juser@picotte001 ~]$ module load picotte-openmpi/gcc`
`    [juser@picotte001 ~]$ python3 -m pip install --user mpi4py`

<strike>

Dynamic Process Management
--------------------------

If you wish to use Dynamic Process Management (DPM), you will probably
need to use MVAPICH2 rather than OpenMPI (or MPICH2). You will need to
use the Hydra process manager, as well. Please check the
[MPI](/MPI "wikilink") article for the specific version of MVAPICH2 to
use. You may need to compile your own MVAPICH2 to have a shared library.

`    [juser@proteusi01 ~]$ module load python/2.7-current`
`    [juser@proteusi01 ~]$ module load proteus-mvapich2/gcc/64/1.9-mlnx-ofed`
`    [juser@proteusi01 ~]$ pip install --user mpi4py`

Then, your job script should do something like:

``` bash
module load proteus-mvapich2/gcc/64/1.9-mlnx-ofed
module load python/2.7-current

MPIEXEC_HYDRA=$( which mpiexec.hydra )
MV2_SUPPORT_DPM=1 $MPIEXEC_HYDRA -np 1 -genvall python2.7 my_computation.py
```

Thanks to J. Wall for working this out. </strike>

See Also
--------

-   [Python](/Python "wikilink")
-   [MPI](/MPI "wikilink")

References
----------

<references/>

[1] [MPI for Python Bitbucket project page](https://bitbucket.org/mpi4py/mpi4py/)