---
title: Exonerate
permalink: /Exonerate/
---

**Exonerate** is a generic tool for pairwise sequence comparison. It
allows you to align sequences using a many alignment models, either
exhaustive dynamic programming or a variety of heuristics.[1]

Installed Versions
------------------

### Picotte

**Exonerate 2.4.0** is installed on Proteus. Use the following
modulefile:

`    exonerate/2.4.0`

### Proteus

**Exonerate 2.4.0** is installed on Proteus. Use the following
modulefile:

`    exonerate/2.4.0`

Running
-------

### Picotte

**exonerate** is multithreaded. Request one (1) node, with &gt; 1
`--cpus-per-task`:

Example:

``` bash
#SBATCH --cpus-per-task=16

exonerate --cores $SLURM_CPUS_PER_TASK
```

### Proteus

**exonerate** is multithreaded. You must request the **shm** PE, with an
appropriate number of slots.

Example snippet:

``` bash
#$ -pe shm 16

exonerate --cores $NSLOTS ...
```

Compiling
---------

Download the source code.

Use this configure command:

`     export CFLAGS="-march=corei7-avx -O3"`
`     ./configure --prefix=/mnt/HA/groups/myGrp/opt/exonerate/2.4.0 --disable-compiledmodels --enable-utilities --enable-pthreads --enable-glib2 `

Then:

`    make`
`    make check`
`    make install`

References
----------

<references/>

[1] [Exonerate web site](https://www.ebi.ac.uk/about/vertebrate-genomics/software/exonerate)