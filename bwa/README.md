**BWA (Burrows-Wheeler Aligner)** is a software package for mapping DNA
sequences against a large reference genome, such as the human genome.[1]

Installed Versions
------------------

### Picotte

**BWA 0.7.17** is installed on Picotte. Use the modulefile:

`   bwa/0.7.17`

Download
--------

Download the latest .tar.gz (aka "tarball") from the SourceForge site
for this project: <http://sourceforge.net/projects/bio-bwa/files/>
Expand the tarball:

`    [juser@proteusa01 src]$ tar zxf bwa-0.n.m.tar.gz`

Alternatively, there seems to be a fork at Github:
<https://github.com/lh3/bwa>

Download the master:

`     [juser@proteusa01 src]$ git clone `[`https://github.com/lh3/bwa`](https://github.com/lh3/bwa)

This creates a directory named bwa. Then, follow the instructions below.

Environment
-----------

Make sure to load the latest gcc:

`    [juser@proteusa01 ~]$ module load gcc`

Modify Makefile and Compile
---------------------------

Update the optimization flags in the Makefile:

`    CFLAGS=     -Wall -O3 -msse4.2 -mavx -mfpmath=sse`

Compile:

`    [juser@proteusa01 bwa-0.n.m]$ make > Make.out 2>&1 &`

Precompiled Version
-------------------

A locally-compiled version, compiled with the Makefile modification
above, can be found in:

`    /mnt/HA/opt/src/bwa-0.7.7`

You may copy that directory structure to your installation location.

Documentation
-------------

Documentation is online in the form of a man page. Do:

`    man bwa`

Running BWA
-----------

BWA can run with multiple threads. If multi-threaded BWA is needed,
request a number of slots equal to the number of threads:

`    #$ -pe shm 16`
`    ...`
`    bwa -t $NSLOTS ...`

References
----------

<references/>

[1] [BWA official website](http://bio-bwa.sourceforge.net/)


References
----------

<references/>

[1] [BWA GitHub repository](https://github.com/lh3/bwa)
