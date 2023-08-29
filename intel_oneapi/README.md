---
title: Intel oneAPI
permalink: /Intel_oneAPI/
---

**Intel oneAPI** is Intel's new compiler suite, based on LLVM, Clang,
and SYCL.[1][2] It provides a unified way to target CPUs, GPUs, and
FPGAs.

An overview is also available in [Intel's oneAPI Fact Sheet](https://newsroom.intel.com/wp-content/uploads/sites/11/2020/11/intel-oneapi-product-fact-sheet.pdf).

Installed on Picotte
--------------------

**oneAPI 2021.3** is installed on Picotte. The following toolkits are
installed:

-   [Base Toolkit](https://software.intel.com/content/www/us/en/develop/tools/oneapi/base-toolkit.html#gs.8sm3z9)
-   [HPC Toolkit](https://software.intel.com/content/www/us/en/develop/tools/oneapi/hpc-toolkit.html#gs.8sm6sd)
-   [AI Analytics Toolkit](https://software.intel.com/content/www/us/en/develop/tools/oneapi/ai-analytics-toolkit.html#gs.8spevu)

oneAPI provides its own modulefile directory. To use oneAPI, first do:

`    module use /ifs/opt/intel/oneapi/2021_3/modulefiles`

Then, do "`module avail`" to see available modules.

Compiler Programs
-----------------

The names of the compiler programs have changed:

-   C/C++ compiler - icx
-   Fortran compiler - ifx

oneAPI also has a "Data Parallel C++" compiler.

Documentation
-------------

Documentation is online.[3]

References
----------

<references/>

[1] [Intel oneAPI website](https://software.intel.com/content/www/us/en/develop/tools/oneapi.html)

[2] [oneAPI specification official website](https://www.oneapi.io/)

[3] [Intel® oneAPI Programming Guide](https://software.intel.com/content/www/us/en/develop/documentation/oneapi-programming-guide/top.html)