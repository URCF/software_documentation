---
title: Vcftools
permalink: /Vcftools/
---

**vcftools** is a program package designed for working with VCF files,
such as those generated by the [1000 Genomes Project](http://www.1000genomes.org/).[1]

Installed Versions
------------------

Use one of the modulefiles:

`    vcftools/20170217`
`    vcftools/20170911`

**N.B.** The newer vcftools/20170911 is multithreaded for possible
performance improvement.

Running
-------

Some of the vcftools are multithreaded, so request the **shm** PE. See
[Writing_Job_Scripts\#Parallel_Environment_.28PE.29](/Writing_Job_Scripts#Parallel_Environment_.28PE.29 "wikilink")

Compiling
---------

### Modules

Load these modules:

``` text
* zlib/cloudflare/gcc/1.2.8
* perl-threaded
* proteus-blas/gcc/64/20110419
* proteus-lapack/gcc/64/3.5.0
```

### Configure and Make

Fairly straightforward. Instructions at the website:
<https://vcftools.github.io/examples.html>

``` bash
export CXXFLAGS="-O3 -march=corei7-avx -pthread"
export CPPFLAGS="-DBGZF_MT -I${ZLIBINC}"
export LDFLAGS="-Wl,-rpath,${ZLIBDIR} -Wl,-rpath,${LAPACKLIBDIR} -Wl,-rpath,${BLASLIBDIR}"
export LIBS="-llapack -lblas -lz"

./configure --prefix=/mnt/HA/groups/myrsrchGrp --enable-largefile --enable-pca
make
```

References
----------

<references/>

[1] [VCFtools website](https://vcftools.github.io/index.html)