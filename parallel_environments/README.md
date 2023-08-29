---
title: Parallel Environments
permalink: /Parallel_Environments/
---

**OBSOLETE**

This article will clarify the purpose of various *parallel environments*
(PEs) for Grid Engine.

Programs which do parallel execution, either using
threads/[OpenMP](/OpenMP "wikilink") or using [MPI](/MPI "wikilink"),
need to request multiple *slots* in a PE. Besides ensuring the proper
number of CPU cores (aka slots) are allocated, a PE may set up the
environment for MPI programs which run on multiple physical nodes.

Threads, OpenMP, Open MPI, MVAPICH2, MPI, Hybrid MPI-OpenMP
-----------------------------------------------------------

All of these are ways to run in parallel. *Running in parallel* means
that more than one operation is performed at the same time.

### Computer CPU Hardware

Please see this brief outline of computer hardware to map the terms here
to the hardware doing the computation:
[Introduction_to_Using_Proteus\#Terminology.2FGlossary](/Introduction_to_Using_Proteus#Terminology.2FGlossary "wikilink")
We summarize that here.

This will be a vastly simplified explanation, which may not match
exactly to reality. Generally speaking, a *compute core* performs one
operation/instruction at a time. This one at a time execution is
**serial**. (There are complications now, e.g. vector operations,
pipelining.) Modern *central processing units* (CPUs, aka *socket*) have
multiple identical compute cores. This allows one CPU to perform
multiple instructions at the same time. Moreover, the nodes in Proteus
have multiple sockets, increasing the number of simultaneous operations
that can be performed.

There is also [Hyper-Threading](/wiki:Hyper-Threading "wikilink"), but
that is not enabled on Proteus nodes because they compromise performance
for compute-intensive (as opposed to memory- or io-intensive) jobs.

### Threads and OpenMP

A *thread* is a sequence of operations. (See [wiki:Thread computing)](/wiki:Thread_(computing) "wikilink") for more detail.)
Threads are software-level objects. The computer's operating system may
run multiple threads, by quickly switching from one to the other. For
instance, one may run more applications at the same time (word
processor, spreadsheet, music player, web browser, etc.) than there are
processor cores. (A typical laptop has 2 to 4 CPU cores.) The operating
system accomplishes this by switching from one to the other. Since these
are all user-oriented programs, and the switching happens quickly, it is
not noticeable to the end-user.

For compute-intensive applications, however, such switching is
detrimental to performance. So, each thread is assigned to one CPU core.
Note, that this is an idealization. The operating system itself (Linux,
Windows, MacOS) has many processes running all the time to manage things
such as network connections, video display, logging, etc.).

**OpenMP** is a specification for a set of computer language extensions
which simplify the task of writing multi-threaded applications.[1]
OpenMP programs are just multi-threaded programs.

A multithreaded program just appears as a single program in Linux: if
you run "ps" or "top", you will only see one process with one process
ID. The CPU usage may be greater than 100%, indicating that the program
is running multithreaded.

### Open MPI, MVAPICH, MPICH, MPI

The **[Message Passing Interface](/Message_Passing_Interface "wikilink")
(MPI)**[2] is a standardized library and Application Programming
Interface (API) for writing parallel programs which can run on multiple
physical computers connected by some networking.[3] As the name implies,
this parallelism is accomplished by separate compute processes which
pass messages to each other. These messages may be bundle up some set of
values, or they may be synchronization messages, or some other types of
messages.

**Open MPI**[4], **MVAPICH**[5], and **MPICH**[6] are implementations of
the MPI standard. Note that OpenMP and Open MPI are completely
unrelated. (However, one may write hybrid MPI-OpenMP or MPI-threaded,
programs.)

MPI programs are run by using the **mpirun** command. The `mpirun`
command launches multiple copies of the "base" program. The number of
copies is user-controllable. The optimal number is one per CPU core.
Each of these copies has their own process ID: if you do a "ps" or "top"
you will see these as separate programs. These separate instances
communicate by passing messages to each other.

### Hybrid MPI-OpenMP

Hybrid MPI-OpenMP programs are those where these individual copies
spawned by `mpirun` are multithreaded. If you choose to use the Open MPI
implementation of MPI, you would have a hybrid Open MPI-OpenMP program.

### Aside on Neural Networks

While the conceptual description of neural networks seems to imply that
each layer "passes a message" to the next, in practice, the entire
network is a single array operation. This is because the "message" from
one layer to the next is merely a vector of floating-point values. So,
neural networks are generally **not MPI**. This is not to say that a
neural network may not be written as an MPI application.

Multithreaded and OpenMP Applications
-------------------------------------

Multithreaded and OpenMP applications, e.g. Python scripts, Perl
scripts, some Matlab scripts, should request the **shm** PE.

### Memory Requests

Treat `m_mem_free` and `h_vmem` as **per-slot** values. So, if you scale
up your job by increasing the number of requested slots, but not
increasing the amount of data each thread handles, you can leave these
values alone.

m_mem_free
a node must have (m_mem_free \* NSLOTS) of memory free in order to run
this job

h_vmem
the job's vmem usage cannot exceed (h_vmem \* NSLOTS)

MPI Applications
----------------

### PEs

Request one of the following PEs:

openmpi_shm
PE for Open MPI jobs which run only on a single node; this is new
(Mar 2017)

openmpi_ib
PE for Open MPI jobs which run on multiple nodes; slots are assigned in
"fill up" order

openmpi_ib_rr
PE for Open MPI jobs which run on multiple nodes; slots are assigned in
"round robin" order

fixedNN
PEs for Open MPI jobs which run on multiple nodes, where the number of
slots per node are fixed at **NN** per node

### Memory Requests

Both `m_mem_free` and `h_vmem` are **per-slot** values

m_mem_free
a node must have (m_mem_free \* no. of slots granted on that node) of
memory free in order to run this job

h_vmem
the job's total vmem usage cannot exceed (h_vmem \* no. of slots
granted on that node)

Hybrid MPI-OpenMP Applications
------------------------------

Hybrid MPI-OpenMP applications should follow the same guidelines as MPI
applications above.

Application-Specific PEs
------------------------

There are applications which require special environment setups for
parallel execution: [Matlab](/Matlab "wikilink"),
[Abaqus](/Abaqus "wikilink"), [STAR-CCM+](/STAR-CCM+ "wikilink"),
[Apache Spark](/Apache_Spark "wikilink"). Please use the appropriate PE
for each.

References
----------

<references/>

[1] [OpenMP FAQ- What is OpenMP?](http://www.openmp.org/about/openmp-faq/#WhatIs)

[2] [wiki:Message Passing Interface](/wiki:Message_Passing_Interface "wikilink")

[3] [MPI Forum](http://mpi-forum.org/) - the standardization forum for
MPI

[4] [Open MPI - open source MPI implementation](https://www.open-mpi.org/)

[5] [MVAPICH - high-performance MPI implementation from Ohio State University](http://mvapich.cse.ohio-state.edu/)

[6] [MPICH - high-performance and portanle MPI implementation](https://www.mpich.org/)