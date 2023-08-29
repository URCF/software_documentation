---
title: OpenMP
permalink: /OpenMP/
---

**OpenMP** is an API that supports multi-platform shared memory
multiprocessing programming in C, C++, and Fortran, on most processor
architectures and operating systems, including Solaris, AIX, HP-UX,
Linux, Mac OS X, and Windows platforms.[1][2]

"Shared memory" means memory which is accessible by all CPU cores,
without going over the network.[3] In practice, this means parallelism
confined within a single execute host/compute node. OpenMP programs
accomplish parallelism exclusively through the use of threads.[4]

N.B. OpenMP is not OpenMPI. OpenMPI is an implementation of the
Message-Passing Interface (MPI), where each process has its own block of
memory, and sharing contents between processes must be done explicitly.
Unfortunately, to add to the confusion, the latest specification of the
MPI standard, MPI-3, includes shared-memory parallelism.

References
----------

<references/>

[1] [Wikipedia article on OpenMP](/Wikipedia:OpenMP "wikilink")

[2] [OpenMP® official website](http://openmp.org/wp/)

[3] [Shared memory article in Wikipedia](http://en.wikipedia.org/wiki/Shared_memory)

[4] [OpenMP Tutorial: Livermore Computing Center](https://computing.llnl.gov/tutorials/openMP/#ProgrammingModel)