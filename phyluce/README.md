---
title: Phyluce
permalink: /Phyluce/
---

**phyluce** is a software package that was initially developed for
analyzing data collected from ultraconserved elements in organismal
genomes.[1]

Installing phyluce
------------------

### Prerequisite

phyluce requires a recent version of [git](/git "wikilink") to run.[2]
Load this module:

`   module load git`

### Java Dependency

phyluce requires Java 1.7.0 which is already installed on all Proteus
nodes. However, the default on Proteus is Java 1.8.0. To set up your
default, add these lines to ~/.bashrc

``` bash
# Use java 1.7.0 as default
export JAVA_HOME=/usr/lib/jvm/java-1.7.0-oracle.x86_64
export PATH=${JAVA_HOME}/bin:${PATH}
```

### Anaconda Python 2.7

phyluce requires an Anaconda-based Python 2.7. The current (2017-10-16)
version 5.0.0.1 works with the workaround of setting up a separate
virtualenv.

### virtualenv

If you now do "conda install phyluce", you will likely get an error
about a conflict with the package "pywavelets" or "bottleneck" or some
others:

&lt;source lang="text&gt; $ conda install phyluce Fetching package
metadata ........... Solving package specifications: .

UnsatisfiableError: The following specifications were found to be in
conflict:

` - phyluce -> biopython 1.63 -> numpy 1.7* -> mkl 10.3`
` - pywavelets`

Use "conda info <package>" to see the dependencies for each package.

</source>
The fix is to set up a separate virtualenv:[3][4]

``` text
### this creates a new virtualenv named "phyluce"
[juser@proteusi01 ~]$ conda create --name phyluce python=2
[juser@proteusi01 ~]$ source activate phyluce
[juser@proteusi01 ~]$ conda install phyluce
# do some work
# when finished
[juser@proteusi01 ~]$ source deactivate
```

### ANACONDA_HOME

Because of this virtual environment, the ANACONDA_HOME environment
variable needs to be adjusted. This should not be done in ~/.bashrc; it
should be done only within the new "phyluce" environment.

``` text
[juser@proteusi01 ~]$ export ANACONDA_HOME=/mnt/HA/groups/myrsrchGrp/Software/anaconda2/envs/phyluce
```

Tutorial
--------

You can test this out using the tutorial:
<https://phyluce.readthedocs.io/en/latest/tutorial-one.html>

The tutorial has other dependencies:

-   illumiprocessor
-   trimmomatic

### illumiprocessor

The illumiprocessor installation will also install trimmomatic:

`    conda install illumiprocessor`

However, illumiprocessor defaults to searching for trimmomatic in
~/anaconda2 If your anaconda is installed elsewhere, you will need to
specify the path to trimmomatic (because it is installed in a
subdirectory of anaconda):

`    illumiprocessor --trimmomatic $ANACONDA_HOME/jar/trimmomatic ....`

To try out the tutorial, you should request an interactive session with
lots of memory because the process consumes up to 148 GB of memory.

`    qlogin -l exclusive -l m_mem_free=3g -l h_vmem=4G -pe shm 64 -l h_rt=4:00:00`

(note the lower case "g" in "m_mem_free=3g" and the upper case "G" in
"h_vmem=4G"). Since the only nodes which have enough free memory to
satisfy the request for 64\*3g ~ 192 GB are the AMD nodes, this will
give an interactive session on an AMD node.

Once the session starts, you must execute your ~/.bashrc manually to set
up your environment:

`    . ~/.bashrc`

If you run the example with 16 cores:

`    illumiprocessor --trimmomatic $ANACONDA_HOME/jar/trimmomatic.jar \`
`       --input raw-fastq/ \`
`       --output clean-fastq \`
`       --config illumiprocessor.conf \`
`       --cores 16`

This memory consumption seems fairly independent of the number of cores
used:

-   4 cores used ~ 146 GB
-   16 cores used ~ 148 GB
-   64 cores used ~ 156 GB

And the run time was similar (within a few seconds) for all three.

Use in Job Scripts
------------------

The customizations above need to be included into job scripts to ensure
a correct environment.

``` bash
#!/bin/bash
#$ -S /bin/bash
...  ### FILL IN THE BLANKS WITH APPROPRIATE
#$ -pe shm 64
#$ -l m_mem_free=3G
#$ -l h_vmem=4G
#$ -l h_rt=12:00:00

. ~/.bashrc

. /etc/profile.d/modules.sh
module load shared
module load gcc
module load sge/univa
module load proteus
module load git

source activate phyluce
export ANACONDA_HOME=/mnt/HA/myrsrchGrp/Software/anaconda2/envs/phyluce

illumiprocessor --input raw-fastq/ --output clean-fastq --config illumiprocessor.conf --trimmomatic $ANACONDA_HOME/jar/trimmomatic.jar --cores $NSLOTS

XXX ...

source deactivate phyluce
```

References
----------

<references/>

[1] [phyluce Documentation](https://phyluce.readthedocs.io/en/latest/)

[2] [phyluce GitHub issue \#59 "OSError", comment by brantfaircloth](https://github.com/faircloth-lab/phyluce/issues/59#issuecomment-249569324)

[3] [phyluce GitHub issue \#57 "bottleneck conflict during install...", comment by brantfaircloth](https://github.com/faircloth-lab/phyluce/issues/57#issuecomment-325659867)

[4] [Conda User guide » Tasks » Managing environments](https://conda.io/docs/user-guide/tasks/manage-environments.html)