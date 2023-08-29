---
title: Stata
permalink: /Stata/
---

**Stata** is data analysis and statistical software.[1]

Installed Versions
------------------

### On Picotte

**Stata 17** is installed. Stata/MP 48-core is installed.

-   Stata 48-core: limited to 10 seats

**NOTE** Please update your job scripts to remove the license request
“`#SBATCH --license=stata:1`” if you do not need it. Current job scripts
with that line may fail, as the license name has been changed to
“`stata48`”.

Load the appropriate modulefile for the version you intend to use:

-   `stata/mp48/17`

### Documentation

PDF documentation is available on Picotte. They are in the directory

`    ${STATADIR}/docs/`

available once the modulefile is loaded.

Documentation is also available online:
<https://www.stata.com/features/documentation/>

Personal Setup
--------------

Your own ADO files go into:[2]

`   ~/ado/personal/`

Running
-------

### Command Line Version

To run the command line version:

``` text
[juser@picotte001 ~]$ module load stata/mp48/17
[juser@picotte001 ~]$ stata-mp
  ___  ____  ____  ____  ____ ®
 /__    /   ____/   /   ____/      17.0
___/   /   /___/   /   /___/       MP—Parallel Edition

 Statistics and Data Science       Copyright 1985-2021 StataCorp LLC
                                   StataCorp
                                   4905 Lakeway Drive
                                   College Station, Texas 77845 USA
                                   800-STATA-PC        https://www.stata.com
                                   979-696-4600        stata@stata.com

Stata license: 10-user 48-core network, expiring 24 Feb 2024
Serial number: 501709314736
  Licensed to: University Research Computing Facility
               Drexel University

Notes:
      1. Unicode is supported; see help unicode_advice.
      2. More than 2 billion observations are allowed; see help obs_advice.
      3. Maximum number of variables is set to 5,000; see help set_maxvar.

.
```

### Jupyter Notebook

See: [Stata in Jupyter](/Stata_in_Jupyter "wikilink")

### Graphical User Interface (GUI) Version

N.B. Running Stata in a Jupyter Notebook (above) will be a more
responsive user experience.

To run the GUI version, you must have an X11 server installed on your
computer. See:

-   [Tips for Windows Users\#Graphical Display to Windows](/Tips_for_Windows_Users#Graphical_Display_to_Windows "wikilink")
-   [Tips for macOS Users\#Graphical Display](/Tips_for_macOS_Users#Graphical_Display "wikilink")

The command to use is **xstata** or **xstata-mp** for the multithreaded
version.

![640px](/XStata.png "wikilink")

Note that if you do this, you will be running Stata on the login node,
which is a shared resource where multiple people may be logged in and
doing work simultaneously.

#### Picotte

To run the both the terminal and GUI versions, see: [Running GUI Applications on Compute Nodes](/Running_GUI_Applications_on_Compute_Nodes "wikilink")

The difference is that for the terminal version, you run **stata-mp**,
while for the GUI version, you run **xstata-mp**

Submitting Jobs on Picotte
--------------------------

For long-running computations, you will want to write a job script to be
submitted as a job on the cluster. Since the limit on the number of CPUs
which may be used simultaneously by any one group is 512, it will be
very easy to exhaust all Stata licenses with job submissions.

Please see more detail in [Writing Slurm Job Scripts](/Writing_Slurm_Job_Scripts "wikilink")

### Requesting License

Only `stata/mp48` jobs require a license. Each job should request one
license. The license on Picotte is limited to no more than 10
simultaneous uses.

``` bash
#SBATCH --licenses=stata48:1
```

N.B. the license name has been changed to ”`stata48`”.

### Stata/MP

[Stata/MP](http://www.stata.com/statamp/), which runs multithreaded, is
also available. It is provided by the command `stata-mp`. There are two
editions of Stata/MP on Picotte: one licensed for 4 cores (unlimited
number of seats), and one licensed for 48 cores (limit of ten seats).

N.B. more cores (threads) does not guarantee better performance. It can
frequently be the reverse.

**NOTE:** you must do "`set processors NN`" in your `.do` file to be the
exact number of slots requested by the job. The number of slots (each
slot is one processor core) is requested by the lines:

``` bash
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=48
#SBATCH --license=stata48:1

module load stata/mp48/17
```

In this example, the number of CPU cores is **48**.

You may also read the environment variable `SLURM_CPUS_PER_TASK` to use
in the "`set processors NN`" command in your Stata `.do` file:

``` text
local p : env SLURM_CPUS_PER_TASK
set processors $p
```

`SLURM_CPUS_PER_TASK` is set by Slurm, the job scheduler, to be the
value requested by "--cpus-per-task".

**NOTE ON PERFORMANCE:** More does not necessarily mean faster. Some
functions/routines may be parallelizable, others may not be. You will
need to benchmark your specific computation to find the optimal number
of CPU cores to use in the computation.

### Example Job for Picotte (Slurm)

This is the Stata script to be run -- named `testing.do`:

``` text
// test computation - testing.do
clear*
set rmsg on
set obs 100000
local p : env SLURM_CPUS_PER_TASK
set processors $p

forval n = 1/5 {
    g i`n' = runiform()
}
g dv = rbinomial(1,.3)
memory

qui logit dv i*

qui xtmixed dv i*

*with bootstrap:
qui bs, reps(2000): logit dv i*
```

This is the job script -- named `teststata.sh`:

``` bash
#!/bin/bash
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=4
#SBATCH --mem=128G
#SBATCH --account=urcfadmprj
#SBATCH --time=4:00:00
#SBATCH --license=stata48:1

module load stata/mp48/17

# set the Stata temporary directory to the job-specific temporary directory
# this directory will be automatically deleted at the end of the job
export STATATMP=$TMP

stata-mp -b do testing.do
```

To submit the job:

`    [juser@picotte001]$ sbatch teststata.sh`

NB you may see a warning/error message in the Slurm output file; this
can be safely ignored.

`   stata-mp: /lib64/libtinfo.so.5: no version information available (required by stata-mp)`

Submitting Jobs on Proteus
--------------------------

**PROTEUS HAS BEEN DECOMMISSIONED** <strike> For long-running
computations, you will want to write a job script to be submitted as a
job on the cluster. Since the limit on the number of CPUs which may be
used simultaneously by any one group is 512, it will be very easy to
exhaust all Stata licenses with job submissions.

Please see more detail on [Writing Job Scripts](/Writing_Job_Scripts "wikilink")

### Stata/MP

[Stata/MP](http://www.stata.com/statamp/), which runs multithreaded, is
also available. It is provided by the command `stata-mp`. The installed
Stata/MP is limited to at most 4 processors.

**NOTE:** you must do "`set processors NN`" in your `.do` file to be the
exact number of slots requested by the job. The number of slots (each
slot is one processor core) is requested by the line:

`    #$ -pe shm 4`

You may also read the environment variable `NSLOTS` to use in the
"`set processors NN`" command. `NSLOTS` is set by Grid Engine, the
scheduler, to be the value requested by "-pe".

### Large Data Files

On Proteus, if you have very large data files, i.e. more than 1 GiB,
please run from the [Lustre Scratch Filesystem](/Lustre_Scratch_Filesystem "wikilink"). Basically:

1.  create your directory in `/scratch`: `mkdir /scratch/myusername`
2.  copy all your files (do file, script, data):
    `cp mystuff.do mystuff.dta myscript.sh /scratch/myusername`
3.  cd into that directory and run there:
    `cd /scratch/myusername ; qsub myscript.sh`
4.  once the job is complete (you should get email notification), copy
    data back to your home

On Picotte, you should instead use the [BeeGFS Scratch File System](/BeeGFS_Scratch_File_System "wikilink") mounted at `/scratch`

### Example Job

You should have a `.do` Stata script ready. The command to run in batch
mode is

`    stata-mp -b do myscript.do`

i.e. specify the "`-b`" option for "batch" mode. This produces a run log
file named "`myscript.log`".

Create a job script file like this, saved as e.g. "`myjob.sh`":

``` text
#!/bin/bash
#$ -S /bin/bash
#$ -P myrsrchPrj
#$ -cwd
#$ -j y
#$ -m bea
#$ -M myname@drexel.edu
#$ -l h_vmem=4g
#$ -l h_rt=0:30:0
#$ -pe shm 4

. /etc/profile.d/modules.sh
module load shared
module load sge/univa
module load gcc/4.8.1
module load proteus
module load stata/13

export STATATMP=$TMP

stata-mp -b do myscript.do
```

**NOTE:** If you create your job script in Windows, you first need to
convert it to Linux format. See: [Tips for Windows Users\#Scripts
Created on
Windows](/Tips_for_Windows_Users#Scripts_Created_on_Windows "wikilink").

Save this as "`myjob.sh`", and then submit to the scheduler:

`    [juser@proteusa01 ~]$ qsub myjob.sh`

Check on your job:

`    [juser@proteusa01 ~]$ qstat`

</strike>

Outputs
-------

Stata, by default, produces a log file named after the `.do` file. So,
running the Stata DO script `something.do` produces the log
`something.log`

If the same DO script is run multiple times, later runs will overwrite
the log from earlier runs.

See Also
--------

-   [Stata Support and Online Resources](https://www.stata.com/support/)
-   If you are interested in converting from Stata to
    [R](/R "wikilink"): <http://dss.princeton.edu/training/> in
    particular this PDF <http://dss.princeton.edu/training/RStata.pdf>
-   [R vs. Stata benchmark](/R_vs._Stata_benchmark "wikilink")

References
----------

<references/>

[1] [Stata official website](http://www.stata.com/)

[2] [STATA FAQs: Personal ADO Directory](http://www.stata.com/support/faqs/programming/personal-ado-directory/)