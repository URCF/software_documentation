---
title: ORCA
permalink: /ORCA/
---

**ORCA** is an ab initio quantum chemistry program package that contains
modern electronic structure methods including density functional theory,
many-body perturbation, coupled cluster, multireference methods, and
semi-empirical quantum chemistry methods. Its main field of application
is larger molecules, transition metal complexes, and their spectroscopic
properties.[1][2][3]

Installed Version
-----------------

**ORCA 5.0.3** is installed. Use the modulefile:

`    orca/5.0.3`

How to Run
----------

N.B. despite using MPI, Orca seems to be able to run on a single-node
only due to the non-standard way it uses MPI. See [this article from the Ohio Supercomputing Center documentation](https://www.osc.edu/book/export/html/5018).

### Example

Input file below. Note the line “`%PAL NPROCS 48 END`” which specifies
that Orca should run in parallel with 48 processes (i.e. MPI ranks).
This is specified separately from the job script
“`--ntasks-per-node=48`” and **must** match that value.

This is unlike normal MPI programs which can run on multiple nodes, and
infer the process distribution from the Slurm resource request.

Example input file (named “`geom.inp`”)

``` text
!B3LYP DEF2-SVP OPT
%PAL NPROCS 48 END
* xyz 0 1
N         -0.83911        0.76325       -0.31843
C          0.61442        0.72014       -0.25075
C          1.01669       -0.56167        0.49740
O          0.20095       -1.36984        0.93753
H         -1.37884        0.05803        0.17605
H          1.00414        0.66192       -1.27362
C          1.17285        1.95192        0.45211
H          0.87124        2.87150       -0.05988
H          0.81191        2.01288        1.48492
H          2.26726        1.92903        0.48485
H         -1.30551        1.57618       -0.71069
O          2.33980       -0.77979        0.66176
H          3.04559       -0.08055        0.28096
*
```

Job script:

``` bash
#!/bin/bash
#SBATCH --time=1:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=48
#SBATCH --time=1:00:00
#SBATCH --mem-per-cpu=3G

module load orca

${ORCADIR}/bin/orca ./geom.inp
```

References
----------

<references/>

[1] [:Wikipedia:ORCA (quantum chemistry program)](/:Wikipedia:ORCA_(quantum_chemistry_program) "wikilink")

[2] [Fast & Accurate Computational Chemistry Tools](https://www.faccts.de/)\]

[3] [ORCA Forum](https://orcaforum.kofo.mpg.de/app.php/portal)
