---
title: Nextflow
permalink: /Nextflow/
---

**Nextflow** is a domain-specific language (DSL) for scalable and
reproducible scientific workflows.[1]

Use Cases
---------

Nextflow is best suited for:

-   workflows which contain significant complexity in the form of
    different applications, data formats, repetition, conditional
    branching, and/or dependencies between all of these
-   workflows which have low maximum scaling (i.e. low total number of
    individual tasks and/or nodes required)
-   workflows which do not require fast turnaround times
-   pre-existing workflows implemented in Nextflow, e.g. downloaded from
    [nf-core](https://nf-co.re/)

Installed Versions
------------------

### Picotte

**Nextflow** (multiple versions) is installed. Use the appropriate
modulefile:

`    nextflow/21.04.0`
`    nextflow/21.04.6`
`    nextflow/21.10.6`

Installing Your Own
-------------------

Nextflow is a single executable that you can install yourself to any
convenient location. Follow the instructions in the Nextflow
documentation.[2]

Running Nextflow
----------------

### General Notes

Nextflow may download a large amount of data, both in the form of
Singularity container images and data as input the the workflow. Due to
the space limitation on home directories (64 GiB), Nextflow workloads
should be run either from the group directory, e.g.
`/ifs/groups/myrsrchGrp/something/nextflow/`, or in a manually-created
subdirectory of the BeeGFS scratch directory, e.g.
`/beegfs/scratch/myusername/nextflow/`.

N.B. The BeeGFS filesystem is a "scratch" filesystem. "Scratch" means
temporary (like "scratch paper" used for writing notes and calculations
apart from your main notebook or homework). Files which have not been
accessed for 45 days or more will be deleted with no possibility of
recovery. So, any outputs which are to be retained should be copied back
to the group directory at the end of the job script.

### Executors

In the Nextflow framework architecture, the executor is the component
that determines the system where a pipeline process is run and
supervises its execution.[3]

Executors which may be used on Picotte include "local" and "slurm".

### Singularity Containers

[Singularity](/Singularity "wikilink") is a container technology used in
HPC. It does not require administrative privileges to run.

Nextflow can use Singularity containers.[4]

For example:

``` text
$ nextflow run my_workflow.nf -with-singularity my_container.sif
```

Published workflows may have a `singularity` profile option:

``` text
$ nextflow run nf-core/mag -profile test,singularity > OUTPUT.txt 2>&1
```

### Local Executor in a Batch Job

The local executor is used by default. It runs the pipeline processes in
the computer where Nextflow is launched.[5]

Write a normal batch job script, and include the "`nextflow ...`"
commandline in it:

``` bash
#!/bin/bash
#SBATCH --partition=def
#SBATCH --time=2:00:00

module load nextflow

### nextflow creates a working directory named "work" in the same directory where it was invoked
nextflow run hello
```

Note that the Nextflow documentation says that “\[t\]he processes are
parallelised by spawning multiple threads and by taking advantage of
multi-cores architecture provided by the CPU”. This may not be strictly
accurate. Nextflow may launch multiple simultaneous independent
processes (i.e. in Linux, each has its own process ID number), each of
which may or may not be multithreaded.

To have Slurm allocate some number of CPU cores to the job, do the
following. See sbatch documentation[6] (or man page).

``` bash
### The default value of "--nodes" if left unspecified is 1 (one)
### Slurm will allocate 48 CPU cores in this instance.
#SBATCH --ntasks=48
```

Important note: bioinformatics workflows may or may not run over
multiple nodes (i.e. individual servers); most do NOT: check the
requirements and limitations of the specific workflow you intend to run.

### Slurm Executor

**WARNING** Left to default configuration, a running Nextflow workflow
manager process can generate a disruptive number of communication
requests to Slurm.

“The slurm executor allows you to run your pipeline script by using the
Slurm resource manager. Nextflow manages each process as a separate job
that is submitted to the cluster by using the sbatch command.“[7]

This config should reduce the frequency of the Slurm requests -- name it
"`nextflow.config`" in your Nextflow work directory:

``` javascript
process {
    executor = 'slurm'
    queueSize = 5
    pollInterval = '5 min'
    dumpInterval = '6 min'
    queueStatInterval = '5 min'
    exitReadTimeout = '13 min'
    killBatchSize = 30
    submitRateLimit = '20 min'
    clusterOptions = '-t 00:30:00'
}
```

See Also
--------

-   [National Energy Research Scientific Computing Center documentation:
    Nextflow](https://docs.nersc.gov/jobs/workflow/nextflow/)
-   [nf-core/mag](/Nf-core-mag "wikilink")

References
----------

<references/>

[1] [Nextflow website](https://www.nextflow.io/)

[2] [Nextflow Documentation
(latest)](https://www.nextflow.io/docs/latest/index.html)

[3] [Nextflow documentation -
Executors](https://www.nextflow.io/docs/latest/executor.html)

[4] [Nextflow Documentation - Singularity
containers](https://www.nextflow.io/docs/latest/singularity.html)

[5] [Nextflow documentation - Executors -
Local](https://www.nextflow.io/docs/latest/executor.html#local)

[6] [Slurm documentation -
sbatch](https://slurm.schedmd.com/sbatch.html)

[7] [Nextflow documentation - Executors -
slurm](https://www.nextflow.io/docs/latest/executor.html#slurm)