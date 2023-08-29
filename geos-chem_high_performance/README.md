---
title: GEOS-Chem High Performance
permalink: /GEOS-Chem_High_Performance/
---

**GEOS-Chem High Performance** (GCHP) is a global 3-D model of
atmospheric chemistry driven by meteorological input from the Goddard
Earth Observing System (GEOS) of the [NASA Global Modeling and Assimilation Office](https://gmao.gsfc.nasa.gov/).[1][2]

Installed Versions
------------------

**GCHP 13.4.0** is installed. Use the modulefile:

`    gchp/intel/2020/13.4.0`

### Usage

Consult the official documentation.[3]

Using GCHP
----------

A “`run`” directory has to be copied to your own working directory, and
then configured for a run.

First, copy the original GCHP run directory:

``` text
$ cd /ifs/groups/myrsrchGrp/myname
$ cp -R $GCHPROOT/run .
$ cd run
$ cp $GCHPROOT/bin/gchp .
```

Create a run directory[4]

``` text
$ ./createRunDir.sh
```

Create a run configuration by creating a run configuration shell script
from the template:

``` text
$ cp runConfig.sh.template runConfig.sh
$ ... edit runConfig.sh ...
```

External Data
-------------

External data is read-only, and a local copy is in:

`    /beegfs/GEOSChemExtData/`

If updates are required, please contact urcf-support@drexel.edu

See Also
--------

-   [GEOS-Chem Classic](/GEOS-Chem_Classic "wikilink")
-   [Compiling GEOS-Chem High Performance](/Compiling_GEOS-Chem_High_Performance "wikilink")

References
----------

<references/>

[1] [GEOS-Chem Website](https://geos-chem.seas.harvard.edu/)

[2] [GEOS-Chem High Performance GitHub Repository](https://github.com/geoschem/GCHP)

[3] [GCHP User Guide (latest version)](https://gchp.readthedocs.io/en/latest/)

[4] [GCHP User Guide - Creating a Run Directory](https://gchp.readthedocs.io/en/latest/user-guide/rundir-init.html)