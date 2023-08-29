---
title: Ray
permalink: /Ray/
---

**Ray** is a de novo assembler which uses MPI for parallel
processing.[1]

Locally Installed Version
-------------------------

Proteus provides **Ray 2.3.1** with the following module file:

`   ray/gcc/2.3.1`

It requires the following modulefiles to run:

`   gcc`
`   proteus-openmpi/gcc/64/1.8.1-mlnx-ofed`

Instructions for compiling it yourself follow.

Download
--------

Download the latest version from the download page.[2] We suggest using
the SourceForge download site since it clearly tags the release version.

Scan the installation instructions in the files `INSTALL.txt` and
`README.md`.

Configure
---------

Load an appropriate [MPI](/MPI "wikilink") modulefile:

`   module load proteus-openmpi/gcc/64/1.8.1-mlnx-ofed`

### Edit Compilation Flags

Edit the provided `Makefile` to change `CXXFLAGS`:[3] (not strictly
necessary)

`   CXXFLAGS = -O3 -march=corei7-avx -std=c++98 -Wall`

### Edit Install Location Prefix

`   PREFIX = /mnt/HA/groups/myrsrchGrp/apps/Ray-2.3.1`

### Compile

`    [juser@proteusa01 Ray-2.3.1]$ make -j 12 | tee Make.out`

You will see a lot of output. This output is also written to the file
named `Make.out`. Once it is done, you should find the executable
"`Ray`" in that directory. You may optionally strip symbols to make a
smaller executable file:

`    [juser@proteusa01 Ray-2.3.1]$ strip Ray`

Optionally, build the documentation for the code:

`    [juser@proteusa01 Ray-2.3.1]$ cd code`
`    [juser@proteusa01 code]$  doxygen DoxygenConfigurationFile`

### Install

`   [juser@proteusa01 Ray-2.3.1]$ make install | tee Make.install.out`
`   `
`   Installing Ray to /mnt/HA/groups/myrsrchGrp/apps/Ray-2.3.1`

### Set Up Environment

Either write an [environment module](/Environment_Modules_Quick_Start_Guide "wikilink"), or modify
your `PATH` environment variable.

Example Job Script
------------------

See the example in `/mnt/HA/examples/Ray/`: documented at [Job Script Example 04 Ray assembler](/Job_Script_Example_04_Ray_assembler "wikilink")

References
----------

<references/>

[1] [Ray -- parallel genome assembler using MPI](http://denovoassembler.sourceforge.net/)

[2]

[3] [Compiling with GCC](/Compiling_with_GCC "wikilink")