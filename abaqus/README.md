---
title: Abaqus
permalink: /Abaqus/
---

**Abaqus**[1] from Dassault Systemes is installed on Proteus.

It makes use of Intel Compilers to compile certain modules at run time.

Installed Versions
------------------

### Picotte

**Abaqus 2021** and **2022** are installed.

To use, load the appropriate modulefile:

`    abaqus/2021`
`    abaqus/2022`

N.B. Abaqus 2021 is not officially supported on Red Hat Enterprise Linux
8. Official support for this OS is not expected until Q4 2021.

### Proteus

OBSOLETE - PROTEUS HAS BEEN RETIRED

Documentation
-------------

No documentation is installed on Picotte. Please see online
documentation here: <http://mem.drexel.edu/abaqus/>

### Official Support

Documentation and knowledge base is available on the web. You will need
a "3Dexperience" account to access these.

-   <https://help.3ds.com/>
-   <https://support.3ds.com/knowledge-base/>

An account is required to access this. Create one here:

`   `[`https://eu1-ds-iam.3dexperience.3ds.com/login?service=https%3A%2F%2Fwww.3ds.com%2F3dexperience`](https://eu1-ds-iam.3dexperience.3ds.com/login?service=https%3A%2F%2Fwww.3ds.com%2F3dexperience)

Special Notes
-------------

### Output Requirements

-   Abaqus generates very large amounts of i/o calls which overload the
    main fileserver providing group and home directories.
-   Please use local scratch for your Abaqus outputs. See the example
    script below.

### Fortran Compilers

-   Abaqus 2021 and 2022 can use GNU Fortran or Intel Fortran:
    -   For GNU Fortran (gfortran), load the modulefile `gcc/9.2.0`
        (this is usually loaded by default for all users)
    -   For Intel Fortran (ifort), load modulefile
        `intel/composerxe/2020u4`
-   If you find compilation errors using ifort, you can modify
    `abaqus_v6.env` to change "ifort" to "gfortran".

Licenses
--------

See license status on the web:

`    `[`https://picottemgmt.urcf.drexel.edu/lmstat.html`](https://picottemgmt.urcf.drexel.edu/lmstat.html)

There are two components to using Abaqus licenses:

-   inclusion in the "abaqususers" group on Picotte
    -   if the error message you see is that the "abaqus" executable
        cannot be found, contact urcf-support@drexel.edu
-   inclusion in the license server configuration, managed by CoE CTS
    -   if you get other license-related errors in your job, contact CoE
        CTS to make sure you are included in the license server
        configuration

**NOTE** the number of CPU cores you can use for Abaqus is determined by
the license your PI has acquired.

OBSOLETE

<strike>Or see number of available licenses for Proteus:

`    [juser@proteusi01 ~]$ qstat -F abaqus`

There is also the "Abaqus Extended" feature:

`    [juser@proteusi01 ~]$ qstat -F abaqus_extended`

### Requesting Licenses with Job

**This is only for Proteus** Picotte does not currently have license
integration like this.

The number of license tokens *M* required for an Abaqus job is given by
the formula

`    `*`M`*` = int(5 x `*`N`*`^0.422)`

where *N* is the number of slots (CPU cores) needed for the job. Request
license tokens with:

`    #$ -l abaqus=`*`M`*

To see how many tokens are available:

`    [juser@proteusi01 ~]$ qstat -F abaqus`

For typical numbers of slots, these are the number of tokens required:

| No. of Slots (N) | No. of Tokens (M) |
|------------------|-------------------|
| 1                | 5                 |
| 4                | 8                 |
| 8                | 12                |
| 16               | 16                |
| 32               | 21                |
| 64               | 28                |
| 128              | 38                |

The Abaqus Extended product is separate. There are only 8 licenses
available in total. </strike> See [Licensed
Software](/Licensed_Software "wikilink") for more information.

Example Job Scripts
-------------------

### Picotte

License token checkout integration is not currently available.

N.B. in the following example, "cpus-per-task" may have to be modified
based on the license you have access to.

``` bash
#!/bin/bash
#SBATCH --partition=def
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=48
#SBATCH --account=myaccountPrj
#SBATCH --mem=128GB
#SBATCH --time=1:00:00

module load abaqus/2021

### $SLURM_NTASKS is automatically substituted from the ntasks-per-node above; do not put in a literal number

abaqus job=cfaxIso cpus=$SLURM_CPUS_PER_TASK interactive
```

### Proteus

**PROTEUS HAS BEEN RETIRED**

IMPORTANT: The job script must be in Unix format. If you wrote the
script on a Windows machine, use the `dos2unix` utility to convert the
file format. See [Tips for Windows Users\#Scripts Created on
Windows](/Tips_for_Windows_Users#Scripts_Created_on_Windows "wikilink")

UPDATE 2019-06-24: Do not use the "abaqus" PE. Use the "fixed16" PE,
instead. Along with that replace "-l vendor=intel" with "-l
ua=sandybridge".

Please see the article on [Writing Job
Scripts](/Writing_Job_Scripts "wikilink") for details on what goes into
a job script.

Parameters:

-   h_rt = wallclock time limit (hh:mm:ss) - job is killed if it runs
    for longer than h_rt
-   h_vmem = hard virtual memory limit - job is killed if it tries to
    use more than h_vmem per slot (CPU core)
-   m_mem_free = minimum free memory per slot (CPU core) needed in
    order for job to run

``` bash
#!/bin/bash
#$ -S /bin/bash
#$ -cwd
#$ -j y
#$ -M myname@drexel.edu
#$ -P myrsrchPrj
#$ -R y
#$ -pe fixed16 64
#$ -l abaqus=28
#$ -l ua=sandybridge
#$ -l h_rt=00:15:00
#$ -l h_vmem=4G
#$ -l m_mem_free=3G
#$ -q all.q

. /etc/profile.d/modules.sh

### These three modules must ALWAYS be loaded
module load shared
module load proteus
module load sge/univa

### These two modules must be loaded for Abaqus
module load intel/composerxe/2013.3.174
module load abaqus/6.13

### $NSLOTS is automatically substituted from the pe above; do not put in a literal number
# abaqus job=cfaxIso cpus=$NSLOTS interactive


### EXAMPLE OF USING LOCAL DISK INSTEAD - uncomment all below and delete the above "abaqus" command
### you will need to copy your .inp file, and any other input files to the TMP directory
cp cfaxIso.inp $TMP
cd $TMP
abaqus job=cfaxIso cpus=$NSLOTS interactive

### copy all outputs back to the original direvtory
cp -f * $SGE_O_WORKDIR
```

Checkpoint and Restart
----------------------

Abaqus has a built-in checkpoint and restart feature.[2][3]

Add the following to the input file (refer to official Abaqus
documentation for detail):

`   *RESTART, WRITE, OVERLAY, FREQUENCY=10`


OVERLAY

saves only one state, i.e. overwrites the restart file every time new restart information is written
FREQUENCY=*N*

writes restart information every *N* timesteps

And, to restart the job, create a new input file *newJobName* with only
a single line:

`   *RESTART, READ`

and run Abaqus specifying both the new and old job names:

`   abaqus jobname=`*`newJobName`*` oldjob=`*`oldJobName`*

Benchmark
---------

Abaqus provides benchmark tests in the Abaqus installation itself. Use
the Abaqus "`fetch`" command to get the appropriate input files and
subroutines. See documentation:

[`http://www.oulu.fi/tietohallinto/unix/abaqus_docs/v6.13/books/bmk/default.htm`](http://www.oulu.fi/tietohallinto/unix/abaqus_docs/v6.13/books/bmk/default.htm)

See Also
--------

-   [Technology Guide for Users of Abaqus.pdf](/SGI_Technology_Guide_for_Users_of_Abaqus.pdf "wikilink")
-   [Compiling for Intel with Intel Composer XE, MKL, and Intel MPI](/Compiling_for_Intel_with_Intel_Composer_XE,_MKL,_and_Intel_MPI "wikilink")
-   [Tips for Windows Users\#Scripts Created on Windows](/Tips_for_Windows_Users#Scripts_Created_on_Windows "wikilink")

References
----------

<references/>

[1] [Abaqus official
website](http://www.3ds.com/products-services/simulia/portfolio/abaqus/overview/)

[2] [Abaqus 6.9 User's Manual - 9.1.1 Restarting an
Analysis](http://abaqusdoc.ucalgary.ca/v6.9/books/usb/default.htm?startat=pt04ch09s01aus46.html)

[3] [Univ. of Leeds Research Computing - Abaqus -
Checkpointing](http://arc.leeds.ac.uk/software/applications/abaqus/#Checkpointing)
