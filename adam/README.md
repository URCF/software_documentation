---
title: ADAM
permalink: /ADAM/
---

**ADAM** provides both an application programming interface (API) and a
command line interface (CLI) for manipulating genomic data on a
computing cluster.[1] It is part of the Big Data Genomics project.[2]

**NOTE** ADAM is *beta* software.

Installed Versions
------------------

ADAM 0.17.0 (using Scala 2.10) and ADAM 0.22.0 (using Spark 2.x and
Scala 2.11) are installed on Proteus. Use one of the modulefiles:

`   adam/0.17.0`
`   adam/0.22.0`

Usage
-----

Job scripts should follow the requirements for [Apache
Spark](/Apache_Spark "wikilink") jobs. Here is an example job script:

``` bash
#!/bin/bash
#$ -S /bin/bash
#$ -P myrsrchPrj
#$ -j y
#$ -cwd
#$ -M myname@drexel.edu
#$ -jc spark.amd
#$ -pe spark.amd 128
#$ -l vendor=amd
#$ -l exclusive
#$ -l h_rt=0:30:00
#$ -l h_vmem=4g
#$ -l m_mem_free=3g

. /etc/profile.d/modules.sh
module load shared
module load gcc
module load sge/univa
module load proteus
module load adam/0.17.0


###
### Set up environment for Spark job
###
export SPARK_CONF_DIR=${SGE_O_WORKDIR}/conf.${JOB_ID}
. ${SPARK_CONF_DIR}/spark-env.sh

./jenkins-test.proteus.sh
```

Example
-------

See:

-   [Job Script Example 07 ADAM](/Job_Script_Example_07_ADAM "wikilink")
-   [Job Script Example 08 ADAM](/Job_Script_Example_08_ADAM "wikilink")

Building ADAM
-------------

You may wish to build ADAM yourself to keep up with ongoing
developments. If you do, select either a release or a branch which uses
Scala 2.10.x -- releases are usually named something like
"`0.17.0_2.10`" with the Scala version dependency appended to the ADAM
version string.

Building will require Maven &gt;= 3.1, and the appropriate [Apache Spark](/Apache_Spark "wikilink") version:

`   [juser@proteusa01 adam-adam-parent-2.10-0.17.0]$ module load apache/maven`
`   [juser@proteusa01 adam-adam-parent-2.10-0.17.0]$ module load apache/spark/1.2.0`
`   [juser@proteusa01 adam-adam-parent-2.10-0.17.0]$ mvn clean package | tee Maven.package.out`

### Testing

To integrate with the Spark installation on Proteus, the `jenkins-test`
script needs to be modified. Please see the example in
`/mnt/HA/examples/ADAM/jenkins/`.

References
----------

<references/>

[1] [Big Data Genomics ADAM Project Website](http://bdgenomics.org/projects/adam/)

[2] [Big Data Genomics official website](http://bdgenomics.org/)