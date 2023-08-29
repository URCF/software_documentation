---
title: BEAGLE
permalink: /BEAGLE/
---

**BEAGLE** is a high-performance library that can perform the core
calculations at the heart of most Bayesian and Maximum Likelihood
phylogenetics packages.[1][2]

NB This BEAGLE does not seem to be the same as [this Beagle by B. Browning](https://faculty.washington.edu/browning/beagle/beagle.html).

Installed Versions
------------------

The version built from source code checked out on 2018-03-19 is
installed on Proteus. It should run on both AMDs and Intels. (NB this is
only a library, and you must use software that links to this library in
order to actually run any applications with BEAGLE.)

Use this modulefile:

`    beagle-lib/gcc/20180319`

Compiling
---------

### Getting the Source Code

The GitHub repository for the code is authoritative:
<https://github.com/beagle-dev/beagle-lib>

However, the "released" versions are out of date. Clone the current
version:

`   git clone `[`https://github.com/beagle-dev/beagle-lib`](https://github.com/beagle-dev/beagle-lib)

This creates a directory named "beagle-lib" containing all the source
code.

### Build Platform

In order that the resulting executable(s) and libraries will work on all
systems in Proteus, this package should be built on `proteusa01` (the
AMD node). Otherwise, the configuration script will pick up the Intel
CPU which has later hardware code features than the AMDs, resulting in
executables which will not run on the AMD nodes.

### Pre-configure

Requires newer versions of autoconf and automake in order to run the
`autogen.sh` script. Load the following modules

`    autoconf`
`    automake`

### Modify the autogen.sh script

Replace the contents of the `autogen.sh` script with the following, i.e.
add the "libtoolize" line:

``` bash
#!/bin/sh
mkdir -p config
libtoolize
autoreconf --force --install -I config -I m4
```

Then, run the script:

`   ./autogen.sh`

### Configure

Define the prefix to be your preferred location for installation. See
the article on [Installing Software for Your Research Group](/Installing_Software_for_Your_Research_Group "wikilink")

`   ./configure --prefix=/mnt/HA/wherever --enable-openmp --enable-sse --enable-avx  > Configure.out 2>&1 &`

### Build and Install

Do:

`   make`
`   make install`

The installation will create two directories containing beagle-lib:

`   ${PREFIX}/include`
`   ${PREFIX}/lib`

You will have to modify the CPPFLAGS and LDFLAGS for any dependent
software to point it to the include and lib directories above. The best
way to this is by writing your own modulefile.[3] You can also set them
in ~/.bash_profile or set them ad hoc at the command line.

References
----------

<references/>

[1] [beagle-lib repository on GitHub](https://github.com/beagle-dev/beagle-lib)

[2] Daniel L. Ayres, et al. BEAGLE: An Application Programming Interface
and High-Performance Computing Library for Statistical Phylogenetics,
Systematic Biology, Volume 61, Issue 1, 1 January 2012, Pages 170–173
<DOI:%5Bhttps://doi.org/10.1093/sysbio/syr100> 10.1093/sysbio/syr100\]

[3] [Environment Modules Quick Start Guide\#Creating_Your_Own_Module_Files]( Environment_Modules_Quick_Start_Guide#Creating_Your_Own_Module_Files "wikilink")