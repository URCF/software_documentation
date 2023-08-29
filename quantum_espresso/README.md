---
title: Quantum Espresso
permalink: /Quantum_Espresso/
---

**Quantum Espresso** is an integrated suite of Open-Source computer
codes for electronic-structure calculations and materials modeling at
the nanoscale. It is based on density-functional theory, plane waves,
and pseudopotentials.[1]

Installed Versions
------------------

**Quantum Espresso 7.1** is installed on Picotte.

CPU-only: use the modulefile

`    quantumespresso/intel/2020/7.1`

Running
-------

Note:

-   Cannot run multi-node hybrid MPI-OpenMP for unknown reason, so
    `cpus-per-task` can only be `1` (one), the default value.
-   Running single-node hybrid MPI-OpenMP is OK. Issue seems to be the
    UCX low-level communications library and/or Infiniband
    hardware/drivers.

### Example Job Script

``` bash
#!/bin/bash
#SBATCH --partition=def
#SBATCH --nodes=3
#SBATCH --tasks-per-node=48
#SBATCH --mem-per-cpu=3G
#SBATCH --time=1:00:00

module load quantumespresso/intel/2020

${MPI_RUN} pw.x < something.in
```

See Also
--------

-   [Compiling Quantum Espresso](/Compiling_Quantum_Espresso "wikilink")

References
----------

<references/>

[1] [Quantum Espresso website](https://www.quantum-espresso.org/)