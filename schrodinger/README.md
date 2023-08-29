---
title: Schrodinger
permalink: /Schrodinger/
---

**Schrodinger**[1] is installed privately for research groups who have a
license.

Installed Version
-----------------

**Schrodinger 2017-1** is installed.

To use, first add your group's module directory

`   [juser@proteusi01 ~]$ module use --append /mnt/HA/groups/myrsrchGrp/opt/modulefiles`

Then load:

`   [juser@proteusi01 ~]$ module load schrodinger/2017-1`

License
-------

Request licenses for specific features by requesting the appropriate
Grid Engine *complex*, for example ("glide_main" is the complex):

`    #$ -l glide_main=16`

These are the features licensed. The complex is just the feature name,
converted into all lower-case:

``` text
Users of CANVAS_ELEMENTS:  (Total of 200 licenses issued;  Total of 0 licenses in use)
Users of CANVAS_MAIN:  (Total of 50 licenses issued;  Total of 0 licenses in use)
Users of CANVAS_SHARED:  (Total of 2000 licenses issued;  Total of 0 licenses in use)
Users of FFLD_OPLS_MAIN:  (Total of 1 license issued;  Total of 0 licenses in use)
Users of KNIME_MAIN:  (Total of 50 licenses issued;  Total of 0 licenses in use)
Users of LIGPREP_MAIN:  (Total of 17 licenses issued;  Total of 0 licenses in use)
Users of MAESTRO_MAIN:  (Total of 2000 licenses issued;  Total of 0 licenses in use)
Users of MMLIBS:  (Total of 4500 licenses issued;  Total of 0 licenses in use)
Users of MMOD_CONFGEN:  (Total of 25 licenses issued;  Total of 0 licenses in use)
Users of MMOD_MACROMODEL:  (Total of 18 licenses issued;  Total of 0 licenses in use)
Users of MMOD_MBAE:  (Total of 9 licenses issued;  Total of 0 licenses in use)
Users of PHASE_DBCREATE:  (Total of 5 licenses issued;  Total of 0 licenses in use)
Users of PHASE_DBSEARCH:  (Total of 5 licenses issued;  Total of 1 license in use)
Users of PHASE_FEATURE:  (Total of 5 licenses issued;  Total of 0 licenses in use)
Users of PHASE_PARTITION:  (Total of 5 licenses issued;  Total of 0 licenses in use)
Users of PHASE_QSAR:  (Total of 5 licenses issued;  Total of 0 licenses in use)
Users of PHASE_SCORING:  (Total of 5 licenses issued;  Total of 0 licenses in use)
Users of PHASE_SELECTIVITY:  (Total of 5 licenses issued;  Total of 0 licenses in use)
Users of SUITE_01JUL2014:  (Total of 16 licenses issued;  Total of 0 licenses in use)
Users of EPIK_MAIN:  (Total of 16 licenses issued;  Total of 0 licenses in use)
Users of GLIDE_MAIN:  (Total of 16 licenses issued;  Total of 0 licenses in use)
Users of GLIDE_SP_DOCKING:  (Total of 16 licenses issued;  Total of 0 licenses in use)
Users of GLIDE_XP_DESC:  (Total of 16 licenses issued;  Total of 0 licenses in use)
Users of GLIDE_XP_DOCKING:  (Total of 16 licenses issued;  Total of 0 licenses in use)\
Users of IMPACT_MAIN:  (Total of 16 licenses issued;  Total of 0 licenses in use)
Users of PSP_BB:  (Total of 16 licenses issued;  Total of 0 licenses in use)
Users of PSP_FR:  (Total of 16 licenses issued;  Total of 0 licenses in use)
Users of PSP_PLOP:  (Total of 16 licenses issued;  Total of 0 licenses in use)
Users of PSP_PLOP_MEMBRANE:  (Total of 16 licenses issued;  Total of 0 licenses in use)
Users of PSP_RB:  (Total of 16 licenses issued;  Total of 0 licenses in use)
Users of PSP_SKA:  (Total of 16 licenses issued;  Total of 0 licenses in use)
Users of PSP_SSP:  (Total of 16 licenses issued;  Total of 0 licenses in use)
Users of PSP_STA:  (Total of 16 licenses issued;  Total of 0 licenses in use)
Users of PSP_STA_GPCR:  (Total of 16 licenses issued;  Total of 0 licenses in use)
Users of QIKPROP_MAIN:  (Total of 16 licenses issued;  Total of 0 licenses in use)
```

For more information on this license mechanism, see [Licensed Software](/Licensed_Software "wikilink").

References
----------

<references/>

[1] [Schrödinger official web site](https://www.schrodinger.com/)