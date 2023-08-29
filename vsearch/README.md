---
title: VSEARCH
permalink: /VSEARCH/
---

**VSEARCH** is an open and free 64-bit multithreaded tool for processing
metagenomic sequences, including searching, clustering, chimera
detection, dereplication, sorting, masking and shuffling.[1] It is an
alterntive to USEARCH by Robert C. Edgar.[2]

Installed Versions
------------------

VSEARCH 1.1.3, 1.10.0, and 2.7.1 are installed on Proteus. Use one of
the modules:

`    vsearch/gcc/1.1.3`
`    vsearch/gcc/1.10.0`
`    vsearch/gcc/2.7.1`

This also provides a man page. To view (after loading the above
modulefile):

`    [juser@proteusa01 ~]$ man vsearch`

The full documentation plus tests is available in `$VSEARCHDIR`.

Using VSEARCH with Multiple Threads
-----------------------------------

When submitting a job, request the "`shm`" resource with the number of
slots equal to the number of threads required:

`   #$ -pe shm 16`

And then, when running vsearch, specify the number of threads (otherwise
it will try to use all CPU cores):

`   vsearch --threads $NSLOTS ...`

Compiling 1.1.3
---------------

They provide three separate makefiles for three separate versions. One
"plain" (Makefile), one which can read and write gzipped data
(Makefile.ZLIB), and one which can read and write BZ2-compresse data
(Makefile.BZLIB). This article will only deal with the "plain" version,
for now.

Download a tarball and expand it. Note that the distribution tarball
includes executables of many other versions for many other machines. You
can delete them.

### Edit the Makefile and Compile

Modify `src/Makefile` according to this diff:

``` diff
--- Makefile.orig       2015-03-18 05:58:00.000000000 -0400
+++ Makefile    2015-08-10 22:42:28.962047475 -0400
@@ -20,14 +20,16 @@
 # Profiling
 #PROFILING=-g -fprofile-arcs -ftest-coverage
 #PROFILING=-g -pg
-PROFILING=-g
+#PROFILING=-g
+PROFILING=

 # Compiler warnings
 WARN=-Wall -Wsign-compare
 #WARN=-Weverything

 CXX=g++
-CXXFLAGS=-O3 -msse2 -mtune=core2 -Icityhash $(WARN) $(PROFILING)
+#CXXFLAGS=-O3 -msse2 -mtune=core2 -Icityhash $(WARN) $(PROFILING)
+CXXFLAGS=-O3 -march=corei7-avx -Icityhash $(WARN) $(PROFILING)
 LINKFLAGS=$(PROFILING)
 LIBS=-lpthread

@@ -64,4 +66,4 @@
        $(CXX) $(CXXFLAGS) -mssse3 -DSSSE3 -c -o $@ $<

 cpu_sse2.o : cpu.cc $(DEPS)
-       $(CXX) $(CXXFLAGS) -c -o $@ $<
+       $(CXX) $(CXXFLAGS) -msse2 -c -o $@ $<
```

And compile it:

`    [juser@proteusa01 vsearch-1.1.3]$ make -j 8 | tee Make.out`

This creates the executable named **vsearch**.

### Test

This test runs the just-built vsearch in `src/`. The results of the run
using the version installed on Proteus is included below:

``` text
[juser@proteusa01 eval]$ ./eval.sh v
Creating random test set

Running search
vsearch v1.1.3_linux_x86_64, 63.0GB RAM, 16 cores
https://github.com/torognes/vsearch

Reading file ./db.fsa 100%
52576347 nt in 380472 seqs, min 32, max 1875, avg 138
WARNING: 447 sequences shorter than 32 nucleotides discarded.
Indexing sequences 100%
Masking 100%
Counting unique k-mers 100%
Creating index of unique k-mers 100%
Searching 100%
Matching query sequences: 208000 of 208500 (99.76%)
318.97user 0.68system 0:24.15elapsed 1323%CPU (0avgtext+0avgdata 715288maxresident)k
0inputs+37864outputs (0major+62379minor)pagefaults 0swaps

Results
Total queries:               208500
True positives:              193500
False positives:              14500
False negatives:              15000

Recall:                          92.81%
Precision:                       93.03%
False negative rate:              7.19%
False discovery rate:             6.97%
F-score:                         92.92%

Matched (id>=70%;cov>=90%):  154700
Matched percentage:              74.20%
```

### Install

Copy the executable **src/vsearch** to any convenient location in your
PATH.

Compiling 1.10.0
----------------

Version 1.10.0 has a configure script. Despite what the configure
options imply, the resulting executable is not linked with bzip2 or
zlib.

### Configure

`    ./configure CXXFLAGS="-O3 -march=corei7-avx -mtune=corei7-avx" CFLAGS="-O3 -march=corei7-avx -mtune=corei7-avx" --prefix=/mnt/HA/opt/vsearch/gcc/1.10.0 --enable-bzip2 --enable-zlib`

Compiling 2.7.1
---------------

Please see the [official instructions](https://github.com/torognes/vsearch). Despite the
instructions, **sudo/root access is not required**: you just have to set
the prefix to a non-privileged location at the configure step.

### Pre-configure

`    module load autoconf automake`
`    ./autogen.sh`

### Configure

`    ./configure CXXFLAGS="-O3" CFLAGS="-O3" --prefix=/mnt/HA/groups/myrsrchGrp --enable-bzip2 --enable-zlib`

### Install

`    make -j 12 install`

And set your PATH, and MANPATH appropriately.

References
----------

<references/>

[1] [VSEARCH GitHub repository](https://github.com/torognes/vsearch)

[2] [USEARCH officical website](http://www.drive5.com/usearch/)