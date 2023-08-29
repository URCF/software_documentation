---
title: CMAQ
permalink: /CMAQ/
---

**CMAQ (Community Multiscale Air Quality Modeling System (CMAQ)** is a
suite of Fortran 90 programs that work in concert to estimate ozone, PM,
toxic compounds, and acid deposition throughout the troposphere.[1]

Installed Version
-----------------

CMAQ 5.2 is installed on Proteus. Use the modulefile:

`   cmaq/intel/2015/5.2`

This sets the environment variables `CMAQ_HOME`, `CMAQ_DATA`, and paths
to locations of various run scripts (CCTM, MCIP, ICON) and others.

The code is compiled as MPI parallel, as well as multithreaded, linking
to Intel's Math Kernel Library (MKL) for performance.

CMAQ Data
---------

The CMAQ data directory is writeable only by admins. You may redefine
the CMAQ_DATA environment variable to your own group's directory.

Compilation Guide
-----------------

See the article: [Compiling CMAQ](/Compiling_CMAQ "wikilink")

Notes on Running CMAQ
---------------------

-   Each node in job has temp files in $TMP (i.e.
    /local/scratch/$JOB_ID.$TASK_ID.$QUEUE )
    -   Snapshot of total shows about 22 MB around beginning of run
        (64-slot, 4-node)

References
----------

<references/>

[1] [CMAQ Official Web Site](https://www.epa.gov/cmaq)