---
title: Gensim
permalink: /Gensim/
---

**gensim** is a machine learning package (unsupervised semantic
modeling) for [Python](/Python "wikilink").[1]

Running Multicore Parallel
--------------------------

gensim offers a "parallelized version of the Latent Dirichlet
Allocation" model.[2]

The documentation linked above indicates that the optimal number of
workers to request for gensim.model.LdaMulticore() is one less than the
number of available CPU cores. Through some experimentation, this does
not seem to be accurate due to an open issue in the Python
multiprocessing module.[3]

Each worker process spawned by gensim.models.LdaMulticore() actually
runs multithreaded using up to N threads, where N is the number of
actual CPU cores. I.e. this disregards the cgroups[4] restriction to a
subset of CPU cores. So, what happens then is that each worker thread
thinks it has access to all the CPU cores, thus causing up to N^2
oversubscription of the CPU.

Since the underlying issue is in Python, and gensim does not work around
that issue, gensim jobs on Proteus will have to work around this.

### Recommendation

-   Decide in advance which type of node the job should run on:
    -   AMD nodes with 64 cores
    -   Intel nodes with 16 cores (ic01 -- ic21)
    -   Intel nodes with 20 cores (ic22)
-   Request an appropriate PE; if Intel, also request an appropriate
    "microarch" (abbreviated as "ua"):
-   AMD 64 cores:

`   #$ -pe shm 64`

-   Intel 16 cores:

`   #$ -pe shm 16`
`   #$ -l ua=sandybridge`

-   Intel 20 cores:

`   #$ -pe shm 20`
`   #$ -l ua=haswell`

-   In your Python script, set

`    gensim.models.LdaMulticore( ... , workers=1, ... )`

That single worker will run multithreaded using all available CPU cores.

References
----------

<references/>

[1] [gensim official website](https://radimrehurek.com/gensim/about.html)

[2] [gensim documentation - models.ldamulticore – parallelized Latent Dirichlet Allocation](https://radimrehurek.com/gensim/models/ldamulticore.html)

[3] [Python issue 26692 - cgroups support in multiprocessing](https://bugs.python.org/issue26692)

[4] [wikipedia:cgroups](/wikipedia:cgroups "wikilink")