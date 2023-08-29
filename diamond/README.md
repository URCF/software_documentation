---
title: DIAMOND
permalink: /DIAMOND/
---

**DIAMOND** is a new high-throughput program for aligning a file of
short reads against a protein reference database such as NR, at 20,000
times the speed of BLASTX, with high sensitivity.[1][2][3][4]

Installed Versions
------------------

### Picotte

**diamond 2.0.9** and **2.0.15** are installed on Picotte. To use, load
the appropriate module:

`    diamond/2.0.9`
`    diamond/2.0.15`

### Proteus

**PROTEUS HAS BEEN DECOMMISSIONED**

**diamond 0.7.9** and **0.9.28** are installed on Proteus. To use, load
the appropriate module:

`    diamond/gcc/0.9.28`
`    diamond/gcc/0.7.9`

Compiling Version 2.0.9
-----------------------

Builds with GCC 9.2.0. Links with NCBI BLAST libraries. Load the
ncbi-blast module `ncbi-blast/2.11.0`

Set the following Cmake variables

``` bash
BLAST_INCLUDE_DIR = /ifs/opt/ncbi-blast/2.11.0/include
BLAST_LIBRARY_DIR = /ifs/opt/ncbi-blast/2.11.0/lib
```

Change this compile option to point to a needed include dir: &lt;source
lang="bash&gt; CMAKE_CXX_FLAGS =
-I/ifs/opt/ncbi-blast/2.11.0/include/ncbi-tools++

</source>
### Compiling with Intel Compilers

-   Does not seem to work
    -   Multiple errors similar to: incompatible redefinition of macro
        "_mm256_set_m128i"
-   DIAMOND source distro includes a copy of LAPACK & LAPACKE. No way in
    Cmake setup to link to MKL, instead.
    -   With version 2.0.15, Cmake seems to find and link properly to
        MKL.

Compiling Version 0.9.28
------------------------

DIAMOND 0.9.28 uses `cmake`. There is a build script included, but it
requires modification to work.

### Compiler

Use:

`   gcc/4.8.1`

### Download

Download a [release source package](https://github.com/bbuchfink/diamond/releases)

Expand the tar archive:

`   tar xf v0.9.28.tar.gz`

This creates the directory

`   diamond-0.9.28`

### cmake

To use cmake, load the module:

`    cmake/3.12.2`

Then, do the following:

`    cd diamond-0.9.28`
`    mkdir BUILD`
`    cd BUILD`
`    ccmake ..`

In `ccmake`, adjust your build options:

-   make sure the compilers gcc and g++ are specified with full paths:
    `/cm/shared/apps/gcc/4.8.1/bin/gcc` and
    `/cm/shared/apps/gcc/4.8.1/bin/g++`
-   specify any compiler options you want, e.g. for
    architecture-specific optimizations
-   fix the installation prefix to the location you want to install to

Compiling Version 0.7.9
-----------------------

### Download

Download either a [release source package](https://github.com/bbuchfink/diamond/releases), or [clone the development version from github](https://github.com/bbuchfink/diamond)

### Boost Libraries

Diamond depends on the Boost C++ libraries[5] It is available on
Proteus.

Load the following module (which should bring in some other
prerequisites:

`    boost/openmpi/gcc/64/1.57.0`

### Modify Makefile

The Makefile is in the `src` subdirectory. Modify the file in the
appropriate places:

``` make
CFLAGS=-O3 -march=corei7-avx -DNDEBUG
CXXFLAGS=-O3 -march=corei7-avx -DNDEBUG -I$(BOOSTINCLUDEDIR) $(WARN)

...

    LIBS=-Wl,-rpath,$(BOOSTLIBDIR) -L$(BOOSTLIBDIR) -lboost_thread -lboost_system -lboost_timer -lboost_chrono -lboost_iostreams -lboost_program_options

...

#ifeq ($(GCC_VER_GTE42),1)
#CXXFLAGS+=-march=native
#endif
```

Then, compile:

`   make -j 8 >& Make.out`

This will create the `diamond` executable in the `bin` subdirectory of
the top level source directory.

Usage
-----

### Documentation

Official documentation is [online at GitHub](https://github.com/bbuchfink/diamond/wiki)

### General

-   Please make sure to specify number of threads (equal to number of
    requested slots) -- it should do the right thing without this
    option, but specifiying this option ensures that it actually does
    the right thing (NB NSLOTS is an environment variable set by the job
    scheduler for every job that runs):

`    diamond blastx -p $NSLOTS ...`
`    diamond blastp -p $NSLOTS ...`

References
----------

<references/>

[1] [DIAMOND website](http://ab.inf.uni-tuebingen.de/software/diamond/)

[2] [*Nature Methods* **12**, 59–60 (2015) -
<doi:10.1038/nmeth.3176>](http://dx.doi.org/10.1038/nmeth.3176)

[3] [DIAMOND community website (discussion boards)](http://www.diamondsearch.org/index.php)

[4] [DIAMOND GitHub repository](https://github.com/bbuchfink/diamond)

[5] [Boost C++ Libraries](http://www.boost.org/)
