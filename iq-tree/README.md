---
title: IQ-TREE
permalink: /IQ-TREE/
---

**IQ-TREE** is fast and effective stochastic algorithm to infer
phylogenetic trees by maximum likelihood.[1]

Installed Versions
------------------

**IQ-TREE 2.0.6** is installed. Use the modulefile:

`   iqtree/2.0.6`

Example Job Script
------------------

The executable is **iqtree2**. The number of threads must be specified,
and should be equal to the number CPU cores requested in the job.

The following job requests:

-   one (1) node
-   eight (8) CPU cores
-   3 GB of memory per CPU core requested (i.e. 24 GB total)
-   maximum of 8 hours of run time

``` bash
#!/bin/bash
#SBATCH --partition=def
#SBATCH --cpus-per-task=8
#SBATCH --mem-per-cpu=3G
#SBTACH --time=8:00:00

module load iqtree/2.0.6

iqtree2 -nt $SLURM_CPUS_PER_TASK -s example.phy
```

References
----------

<references/>

[1] [IQ-TREE website](http://www.iqtree.org/)