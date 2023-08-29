---
title: OpenFOAM
permalink: /OpenFOAM/
---

**OpenFOAM®** is free, open source software for computational fluid
dynamics (CFD), developed primarily by CFD Direct, on behalf of the
OpenFOAM Foundation.[1] There is also a separate version from the ESI
Group at openfoam.com (instead of .org).[2] We refer to these as
“OpenFOAM.org” and “OpenFOAM.com” respectively.

Installed Versions
------------------

**OpenFOAM.com v2212** is installed. Use the modulefile:

`   openfoam-com/gcc/2212`

This has been compiled with OpenMPI 4.1.4. For information on running
MPI applications, see [Message Passing Interface](/Message_Passing_Interface "wikilink").

ParaView
--------

### “Headless” (i.e. no GUI display)

A headless version of ParaView 5.11.0 is provided with the above
modulefile.

### GUI version

See: [ParaView](/ParaView "wikilink")

See Also
--------

-   [Compiling OpenFOAM](/Compiling_OpenFOAM "wikilink")

References
----------

<references/>
OBSOLETE
========

<strike>

Installed Versions
------------------

OpenFOAM **3.0.x** and **6** are installed on Proteus.

Both OpenFOAM 3.0.x and 6 will work only on Intel nodes.

### OpenFOAM 3.0.x

To use, load these three modules:

`    intel/composerxe/2015.1.133`
`    proteus-openmpi/intel/2015/1.8.1-mlnx-ofed`
`    openfoam/3.0.x`

### OpenFOAM 6

To use, load these modules -- NB **do not** load the Intel Composer XE
module, nor the proteus-openmpi/intel module:

`    proteus-openmpi/gcc/64/1.8.1-mlnx-ofed`
`    openfoam/6`

Due to compatibility issues, OpenFOAM 6 was installed with private
versions of MPI, Python, Perl, and a few other software packages. It was
also compiled with GCC instead of the Intel compilers, but was targeted
for the Intel nodes.

### Using ParaView

ParaView is a graphical program which requires its window to display on
your PC or laptop.

Example Job Script
------------------

OpenFOAM will run only on the Intel nodes. So, the following resource
request is required:

`    #$ -l vendor=intel`

For a parallel job, please see [Message Passing Interface\#Running 2](/Message_Passing_Interface#Running_2 "wikilink")

See [Writing Job Scripts](/Writing_Job_Scripts "wikilink") for more
information on writing job scripts. </strike>


[1] [OpenFOAM® Free Official Web Site](http://www.openfoam.org/)

[2] [OpenFOAM from ESI Group](https://www.openfoam.com/about-esi-opencfd)