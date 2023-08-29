---
title: PETSc
permalink: /PETSc/
---

**PETSc** is is a suite of data structures and routines for the scalable
(parallel) solution of scientific applications modeled by partial
differential equations. It supports MPI, and GPUs through CUDA or
OpenCL, as well as hybrid MPI-GPU parallelism. PETSc (sometimes called
PETSc/Tao) also contains the Tao optimization software library.[1]

Installed Versions
------------------

PETSc is available only on Intel or GPU nodes.

All modulefiles below load the appropriate compiler and Open MPI
version.

### Standard (Sandy Bridge and Ivy Bridge) nodes

The following modules are available:

`    petsc/intel/2013/3.5.3`
`    petsc/intel/2013/3.6.3`
`    petsc/intel/2015/3.12.1`

### New (40-core Skylake) nodes

The following module is available:

`    petsc/intel/2019/3.12.1`

### GPU nodes

The following module is available, linked against CUDA 9.0:

`    petsc/3.12.1`

This is built with GCC 6.2.1 via [Red Hat Software Collections](/Red_Hat_Software_Collections "wikilink")' `devtoolset-6`.

To compile code with this version of PETSc, you will need to run in the
`devtoolset-6` environment. See the Red Hat Software Collections article
for details.

See Also
--------

-   [Compiling PETSc](/Compiling_PETSc "wikilink")

References
----------

<references/>

[1] [PETSc website](https://www.mcs.anl.gov/petsc/)