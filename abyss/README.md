**ABySS** is Assembly By Short Sequences - a de novo, parallel,
paired-end sequence assembler[1][2]

Installed Versions
------------------

ABySS 2.3.5 is available on Picotte. Modulefile:

-   abyss/2.3.5

This is MPI-enabled, compiled with GCC.

Download
--------

Download the source package from the official website:
<http://www.bcgsc.ca/platform/bioinfo/software/abyss>

### Dependencies

Requires:

-   sparsehash[2]

Compiling with GCC
------------------

t.b.a.

Usage
-----

Please consult ABySS documentation.

### Multi-threaded and MPI

Multi-threaded processes run on a single node. MPI jobs run on multiple
nodes.

To run multi-threaded ABySS, your job must request a **shm** "parallel
environment" (PE) with the appropriate number of threads to be run. To
run an MPI version, request **openmpi_ib**, **openmpi_ib_rr**, or
**openmpi_rankfile** PE.

See the article on [Writing Job
Scripts](/Writing_Job_Scripts "wikilink").

References
----------

<references/>

[1] [ABySS official
website](http://www.bcgsc.ca/platform/bioinfo/software/abyss)

[2] [ABySS GitHub repository](https://github.com/bcgsc/abyss)
