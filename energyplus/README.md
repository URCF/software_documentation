---
title: EnergyPlus
permalink: /EnergyPlus/
---

**EnergyPlus™** is a whole building energy simulation program that
engineers, architects, and researchers use to model both energy
consumption—for heating, cooling, ventilation, lighting and plug and
process loads—and water use in buildings.[1]

Picotte
=======

Installed Versions
------------------

**EnergyPlus 9.4.0** is installed on Picotte. Use the modulefile:

`   energyplus/9.4.0 -- documentation in /ifs/opt/energyplus/9.4.0/Documentation/pdf`

N.B. this uses the Intel compiler suite with Intel Python, so it will
conflict with other Python modules.

Proteus
=======

Installed Versions
------------------

**EnergyPlus 9.1.0** for the Intel (Sandy Bridge) nodes, and the new
Intel Skylake nodes is installed. Use the modulefile:

`    energyplus/intel/2019/9.1.0`

Both versions are linked to the Intel Math Kernel Library (MKL) for
optimal perfomance.

### CPU compatibility

Because EnergyPlus is compiled against the Intel MKL, it can run only on
the Intel nodes. In your job script, request:

`   #$ -l vendor=intel`

You could, instead, specify the microarchitecture. However, a more
restrictive resource request like that would reduce the pool of nodes
available to your job, and may result in longer wait times.

Standard Input Files
--------------------

### IDD Files

IDD files are in:

`    ${ENERGYPLUSDIR}/IDD`

### Weather Files

The weather files provided by EnergyPlus are in:

`    ${ENERGYPLUSDIR}/WeatherData`

Notes on Running
----------------

-   EnergyPlus can run multithreaded, but NOT multi-node: only request
    the **shm** PE
-   EnergyPlus recommends placing input files on a local filesystem,
    rather than a shared filesystem
    -   To do so in a job script, copy the inputs to `${TMPDIR}`

Example Job
-----------

Here is an example job script, adapted from the EnergyPlus Quick Start
guide[2]

``` bash
#!/bin/bash
#$ -S /bin/bash
#$ -M MYNAMEHERE@drexel.edu
#$ -cwd
#$ -j y
#$ -P MYPROJECTHERE
#$ -q all.q
#$ -pe shm 16
### NOTE to use the new 40-core Skylake nodes, replace the above 2 lines with:
# #$ -q new.q
# #$ -pe shm 40
#$ -l vendor=intel
#$ -l h_rt=2:00:00
#$ -l m_mem_free=1g
#$ -l h_vmem=2g
. /etc/profile.d/modules.sh
module load shared
module load proteus
### NOTE to use the new 40-core Skylake nodes, replace "module load proteus" with "module load proteus-rh68"
module load sge/univa
module load energyplus/intel/2019/9.1.0

### NOTE This example job copies all inputs to a temporary local directory,
###      generates output in that same directory, and then copis all
###      outputs back to the working (shared) directory. This was the
###      recommendation from EnergyPlus documentation.
###      However, this does not seem to be necessary for small jobs.
###      As the amount of input and/or output data grows, this may
###      be necessary

### Copy all input files to the job-specific temporary directory, given by the environment variable TMPDIR
### NB this directory and its contents will be automatically deleted at end of job
cp $ENERGYPLUSDIR/IDD/Energy+.idd $TMPDIR
cp $ENERGYPLUSDIR/WeatherData/USA_IL_Chicago-OHare.Intl.AP.725300_TMY3.epw $TMPDIR
cp $ENERGYPLUSDIR/ExampleFiles/5ZoneAirCooled.idf $TMPDIR

### Change directory to the TMPDIR (i.e. local directory) and run.
### All output is generated in that directory, so have to copy it back
### to the SGE_O_WORKDIR before it gets automatically deleted
cd $TMPDIR
energyplus -i Energy+.idd -w USA_IL_Chicago-OHare.Intl.AP.725300_TMY3.epw 5ZoneAirCooled.idf
cp eplus* $SGE_O_WORKDIR
```

Documentation
-------------

A local copy of the documentation, in PDF format, is here:

`    ${ENERGYPLUSDIR}/doc`

Online documentation is here:

`    `[`https://energyplus.net/documentation`](https://energyplus.net/documentation)

### Example Files

The example files provided by EnergyPlus are in:

`    ${ENERGYPLUSDIR}/ExampleFiles`

See Also
--------

-   [Compiling EnergyPlus](/Compiling_EnergyPlus "wikilink")

References
----------

<references/>

[1] [EnergyPlus web site](https://energyplus.net)

[2] [EnergyPlus - Quick Start Guide](https://energyplus.net/quickstart)