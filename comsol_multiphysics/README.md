---
title: COMSOL Multiphysics
permalink: /COMSOL_Multiphysics/
---

**COMSOL Multiphysics®** is multiphysics simulation software.[1] Access
to COMSOL Multiphysics is restricted to the license owners.

Installed Versions
------------------

**COMSOL Multiphysics 5.4** is installed on Proteus. Load the
modulefile:

`    comsol/5.4`

Example Job
-----------

An example job script:

``` bash
#!/bin/bash
#$ -S /bin/bash
#$ -P myrsrchPrj
#$ -cwd
#$ -j y
#$ -M myname@drexel.edu
#$ -q all.q
#$ -pe shm 16
#$ -l vendor=intel
#$ -l h_rt=24:00:00
#$ -l m_mem_free=3G
#$ -l h_vmem=4G

. /etc/profile.d/modules.sh
module load shared
module load sge/univa
module load proteus
module load comsol/5.4

comsol batch -inputfile cylinder_flow.mph -outputfile cylinder_flow_result.mph  -batchlog cylinder_flow.log -np $NSLOTS
```

References
----------

<references/>

[1] [COMSOL Website](https://www.comsol.com)