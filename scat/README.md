---
title: SCAT
permalink: /SCAT/
---

**SCAT** software for "Smoothed and Continuous Assignment Tests".[1][2]

Installed Versions
------------------

**SCAT 3.0.2** (compiled from development branch source code) is
installed on Picotte. Use the modulefile:

`   scat/3.0.2`

Running SCAT
------------

The executable is **SCAT3**. For example, using the [test files provided by SCAT](https://github.com/stephens999/scat/tree/development/docs):

``` text
[juser@picotte001 ~]$ module load scat
[juser@picotte001 ~]$ mkdir results
[juser@picotte001 ~]$ SCAT3 test.genotype.txt test.location.txt results 2
```

### Run as a Slurm Job

**SCAT** does not appear to be explicitly multithreaded. Experiment with
various numbers of tasks to see if there is speed-up with more tasks,
indicating some multithreading capability.

``` bash
#!/bin/bash
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --time=12:00:00
#SBATCH --mem=4G

module load scat

# this assumes the directory "results" exists
SCAT3 test.genotype.txt test.location.txt results 2
```

References
----------

<references/>

[1] [SCAT GitHub repository](https://github.com/stephens999/scat)

[2] [Using DNA to track the origin of the largest ivory seizure since the 1989 trade ban, Wasser et al., PNAS **104** 10, DOI: 10.1073/pnas.0609714104](https://doi.org/10.1073/pnas.0609714104)