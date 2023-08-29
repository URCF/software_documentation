---
title: Apache Spark
permalink: /Apache_Spark/
---

**Apache Spark™**[1][2] is a fast and general engine for large scale
data processing ("big data"). It is similar to Hadoop Map-Reduce, but
performs operations in-memory as much as possible. It provides APIs in
Scala[3], [Python](/Python "wikilink"), and [R](/R "wikilink").

To use, load one of the available modulefiles. The following versions
are available on Proteus:

`    apache/spark/2.2.0`
`    apache/spark/1.4.1`
`    apache/spark/1.2.0`

The older 1.2.0 version is specifically used for Big Data Genomics' ADAM
package.[4][5]

Local Installation and Usage
----------------------------

The installation of Proteus runs Spark in stand-alone mode. Due to the
way Spark runs, some additional set up must be done by the user in each
job script. (See the example linked below.) Because of the fairly strict
job requirements compared to normal jobs, a couple of *job classes* are
used to constrain the resources that Spark jobs can request.

Spark can run only in exclusive mode: all slave nodes assigned to the
Spark cluster must be able to use all the CPU cores and all the memory
available. This is because Spark does not integrate well with Grid
Engine.

### Python Interface

Both versions of the Spark 1.x module load Python 2.7. Spark 2.2.0 uses
a private installation of Python.

### Job Class and Parallel Environment

Decide if you want to run on the AMD nodes, or the Intel nodes. For the
AMD nodes, use the `spark.amd` job class (JC), and the `spark.amd`
parallel environment (PE). For Intel nodes, use the `spark.intel` JC,
and the `spark.intel` PE. The JC will constrain all resource requests:
if the job script specifies something that is outside the constraints,
the job will not be accepted.

The snippet for AMD:

``` bash
### Spark on AMD -- no. of requested slots must be multiple of 64
#$ -jc spark.amd
#$ -l exclusive
#$ -pe spark.amd 256
#$ -l vendor=amd
#$ -l h_vmem=4g
#$ -l m_mem_free=3g
```

and the snippet for Intel:

``` bash
### Spark on Intel -- no. of requested slots must be multiple of 16
#$ -jc spark.intel
#$ -l exclusive
#$ -pe spark.intel 32
#$ -l vendor=intel
#$ -l h_vmem=4g
#$ -l m_mem_free=3g
```

For Spark 2.2.0, the appropriate PEs are:

`   spark2.intel`
`   spark2.amd`

### Job Script Setup

In addition, some setup has to be done in the job script before running
any Spark scripts -- see the full example for details:

``` bash
###
### Set up environment for Spark
###
export SPARK_CONF_DIR=${SGE_O_WORKDIR}/conf.${JOB_ID}
. ${SPARK_CONF_DIR}/spark-env.sh
```

### Large Data Files

2017-10-06: Running Spark on Lustre seems to cause Lustre-related
errors, which cause worker procecss to be terminated. So, use /mnt/HA
for data files for the meantime. Watch this space for updates.

~~For large data files which will be accessed by many Spark slaves
concurrently, it will probably be better to run off the [Lustre Scratch Filesystem](/Lustre_Scratch_Filesystem "wikilink") with an appropriately
striped set of input files.~~

Examples
--------

See [Job Script Example 03 Apache Spark](/Job_Script_Example_03_Apache_Spark "wikilink")

Building Spark from Source
--------------------------

All versions require Maven &gt;= 3.1. You may use the one installed on
Proteus:

`    [juser@proteusa01 spark]$ module load apache/maven/3.3.3`

or use the one distributed with the Spark source, located in:

`    spark-source/build/mvn`

If you use the `make-distribution.sh` shell script as described below,
it will default to the bundled version. The "`--name`" option will set
an identifying string to be used as part of the distribution directory
name. The "`--tgz`" option will build a tarball for ease of transferring
to a different location.

### Spark 1.2.x

-   Spark 1.2.0 required for [Big Data Genomics' ADAM](http://bdgenomics.org/projects/adam/). See also:
    [ADAM](/ADAM "wikilink")
-   Flume fails, Kafka fails - build command below excludes these, and
    succeeds in building

`    [juser@proteusa01 spark]$ ./make-distribution.sh --name myname --tgz -DskipTests -pl \!external/flume,\!external/flume-sink,\!external/kafka | tee Make.distribution.out`

### Spark 1.4.1

-   No special options needed:

`    [juser@proteusa01 spark]$ ./make-distribution.sh --name myname --tgz -DskipTests | tee Make.distribution.out`

See Also
--------

-   [Spark the fastest open source engine for sorting a petabyte](https://databricks.com/blog/2014/10/10/spark-petabyte-sort.html)

References
----------

<references/>

[1] [Apache Spark official website](http://spark.apache.org/)

[2] [M. Zaharia, M. Chowdhury, T. Das, A. Dave, J. Ma, M. McCauley, M.J. Franklin, S. Shenker, I. Stoica. Resilient Distributed Datasets: A Fault-Tolerant Abstraction for In-Memory Cluster Computing, NSDI 2012, April 2012](http://www.cs.berkeley.edu/~matei/papers/2012/nsdi_spark.pdf)

[3] [Scala language official website](http://www.scala-lang.org)

[4] [Big Data Genomics ADAM project website](http://bdgenomics.org/projects/adam/)

[5] [ADAM](/ADAM "wikilink") - Proteus installation