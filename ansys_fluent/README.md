---
title: ANSYS Fluent
permalink: /ANSYS_Fluent/
---

**ANSYS Fluent** is computational fluid dynamics software.[1]

Installed Version
-----------------

ANSYS Fluent 2023R1 is installed on Picotte. Use the modulefile:

`    ansysfluent/2023R1`

License
-------

There is a limit of 25 licenses on Picotte. To use ANSYS Fluent in a
job, request an appropriate number of license tokens, e.g.

``` bash
#SBATCH --licenses=fluent:1
```

Interactive Use
---------------

Interactive use requires remote display to your computer. See:

-   [Running GUI Applications on Compute Nodes](/Running_GUI_Applications_on_Compute_Nodes "wikilink")

Noninteractive (Batch) Use
--------------------------

t.b.a.

It seems to be possible to launch a non-interactive job via the GUI:

![512px](/ANSYS_Fluent_GUI_Slurm.png "wikilink")

but better to manually write a job script for easy repeatability.

References
----------

<references/>

[1] [ANSYS Fluids](https://www.ansys.com/products/fluids)