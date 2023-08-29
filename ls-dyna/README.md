---
title: LS-Dyna
permalink: /LS-Dyna/
---

**LS-Dyna** is a gen­er­al-pur­pose fi­nite el­e­ment pro­gram ca­pa­ble
of sim­u­lat­ing com­plex re­al world prob­lems. It is used by the
au­to­mo­bile, aero­space, con­struc­tion, mil­i­tary,
man­u­fac­tur­ing, and bio­engi­neer­ing in­dus­tries.[1]

Installed Versions
------------------

-   LS-Dyna SMP 13.1.1
    -   Use the modulefile "ls-dyna/smp/13.1.1"
-   LS-Dyna HYB 13.0.0
    -   Use the modulefile "ls-dyna/hyb/13.0.0"

Licensing
---------

Use of LS-Dyna is restricted to those groups which have licensed it.

Example Job Script
------------------

The example is taken from
<https://www.dynaexamples.com/icfd/basics-examples/cylinder_flow>

This is an example of a hybrid MPI-SMP (aka hybrid MPI-OpenMP) run,
running on 2 nodes, using 12 MPI ranks per node (a.k.a. processes a.k.a.
tasks) each of which is running 4 threads (each thread on its own CPU
core). (MPI = Message Passing Interface; SMP = Symmetric
Multi-Processing aka threads)

``` bash
#!/bin/bash
#SBATCH --partition=def
#SBATCH --nodes=2
#SBATCH --ntasks-per-node=12
#SBATCH --cpus-per-task=4
#SBATCH --mem=8G
#SBATCH --time=1:00:00

module load ls-dyna/hyb

$MPIRUN ls-dyna ncpu=$SLURM_CPUS_PER_TASK i=i.k
```

### About MPI

For an explanation of how MPI and hybrid MPI-OpenMP programs work,
including a glossary of terms, see: [Hybrid MPI-OpenMP Jobs](/Hybrid_MPI-OpenMP_Jobs "wikilink")

See Also
--------

-   [[File:LS-DYNA®_HYBRID_Studies_using_the_LS-DYNA®_Aerospace_Working_Group_Generic_Fan_Rig_Model.pdf](File:LS-DYNA®_HYBRID_Studies_using_the_LS-DYNA®_Aerospace_Working_Group_Generic_Fan_Rig_Model.pdf)](/LS-DYNA®_HYBRID_Studies_using_the_LS-DYNA®_Aerospace_Working_Group_Generic_Fan_Rig_Model.pdf "wikilink")

References
----------

<references/>

[1] [LS-Dyna website at ANSYS/Livermore Software Technology](https://www.lstc.com/products/ls-dyna)