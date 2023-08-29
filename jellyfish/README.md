---
title: Jellyfish
permalink: /Jellyfish/
---

**Jellyfish** is a k-mer counter.[1]

Picotte
=======

Installed Version
-----------------

**Jellyfish 2.3.0** is installed on Picotte. Use the modulefile:

`    jellyfish/2.3.0 (NB will also load perl-threaded/5.32.1 and python/gcc/3.9.1)`

Running Jellyfish
-----------------

Jellyfish is multithreaded but does not support [MPI](/MPI "wikilink").
Request more than one CPU (i.e. CPU core) per task (one CPU per thread),
e.g.

`    #SBATCH --nodes=1`
`    #SBATCH --cpus-per-task=4`

Proteus
=======

Installed Versions
------------------

There are a few installed versions: 1.1.11, 2.1.3, and 2.2.10. The
installed version 2.1.3 may be buggy as it did not pass all the tests in
its accompanying test suite. The developer has been informed.

To use, load the appropriate module:

`    jellyfish/gcc/1.1.11`
`    jellyfish/gcc/2.1.3`
`    jellyfish/gcc/2.2.10  -- NB will also load perl-threaded/5.26.2 and python/3.6.1`

Running Jellyfish
-----------------

Jellyfish is multithreaded, and does not support [MPI](/MPI "wikilink").
Please use only the `shm` PE, e.g.

`   #$ -pe shm 4`
`   `
`   jellyfish -t $NSLOTS ....`

The **`-t`** option specifies the number of compute threads to run.

Please read the official documentation for details:

-   Version 1:
    <http://www.cbcb.umd.edu/software/jellyfish/jellyfish-manual-1.1.pdf>
-   Version 2: <http://www.genome.umd.edu/docs/JellyfishUserGuide.pdf>

Compiling Jellyfish
-------------------

INCOMPLETE

-   Download source from Github -
    <https://github.com/gmarcais/Jellyfish>
-   Use autotools to generate configure script
    -   aclocal -I .
    -   autoheader
    -   automake --add-missing --copy --force-missing
    -   autoconf
-   Modules

`    zlib/cloudflare/gcc/1.2.8`
`    proteus-blas/gcc/64/20110419`
`    proteus-lapack/gcc/64/3.5.0`
`    gsl/gcc/2.2.1`
`    samtools/1.8`
`    perl-threaded/5.26.2`
`    python/3.6.1`

-   Environment variables:

`   HTSLIB_CFLAGS=-I/mnt/HA/opt/htslib/1.8/include`
`   HTSLIB_LIBS=-L/mnt/HA/opt/htslib/1.8/lib -Wl,-rpath,/mnt/HA/opt/htslib/1.8/lib -lhts`
``    SAMTOOLS=`which samtools` ``

-   configure

`   ./configure --prefix=/mnt/HA/opt/jellyfish/gcc/2.2.10 --with-sse --enable-rpath \`
`        --enable-htslib --disable-python-binding --enable-perl-binding --disable-ruby-binding `

-   make
-   make check

References
----------

<references/>

[1] [Jellyfish mer counter official website](http://www.genome.umd.edu/jellyfish.html) (formerly
<http://www.cbcb.umd.edu/software/jellyfish/>)