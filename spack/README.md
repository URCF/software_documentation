---
title: Spack
permalink: /Spack/
---

**Spack** is a "package manager for supercomputers, Linux, and macOS. It
makes installing scientific software easy."[1] It also allows for
installing multiple versions of the same software, and use of multiple
versions of multiple compiler suites.

Use Case
--------

Spack should be well-suited for installing software that is to be used
by all members of your research group. This should be done to the group
directory, where all group members have access.

### Modulefiles

Spack can create modulefiles for the software that is installed.

Using Spack
-----------

By default, Spack installs software to the same directory where Spack is
installed. You should download and install your own copy of Spack for
your research group; total space required is about 550 MB. (That 550 MB
is just for Spack itself. Installing other software consumes more
space.)

Guides
------

-   [Spack tutorial](https://spack-tutorial.readthedocs.io/en/latest/)
-   [POP-COE.EU - Tool Time: Installing and Managing HPC Software with Spack](https://pop-coe.eu/blog/tool-time-installing-and-managing-hpc-software-with-spack)

References
----------

<references/>

[1] [Spack website](https://spack.io)