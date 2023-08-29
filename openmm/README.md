---
title: OpenMM
permalink: /OpenMM/
---

**OpenMM** is a high performance toolkit for molecular simulation.[1]

Installed Versions
------------------

<strike>**OpenMM 7.3.1** is installed on Proteus. Use the modulefile:

`   openmm/7.3.1`

</strike>

**OpenMM 7.4** using [Anaconda](/Anaconda "wikilink")
[Python](/Python "wikilink") is installed, separately for the standard
queue (`all.q`) and the new queue (`new.q`). The reason these separate
setup methods have to be used is that Anaconda assumes an installation
on a single-user system, as opposed to a shared system for multiple
users and possibly multiple OS versions.

The installations include OpenMMTools,[2] a Python interface for OpenMM.

### Standard Queue all.q

First, create the file `~/.bashrc_conda`:

``` bash
# >>> conda initialize >>>
# !! Contents within this block are managed by 'conda init' !!
__conda_setup="$('/mnt/HA/opt/python/anaconda3/bin/conda' 'shell.bash' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__conda_setup"
else
    if [ -f "/mnt/HA/opt/python/anaconda3/etc/profile.d/conda.sh" ]; then
        . "/mnt/HA/opt/python/anaconda3/etc/profile.d/conda.sh"
    else
        export PATH="/mnt/HA/opt/python/anaconda3/bin:$PATH"
    fi
fi
unset __conda_setup
# <<< conda initialize <<<
```

For interactive use:

``` text
[juser@proteusi01]$ module load python/anaconda3
[juser@proteusi01]$ . ~/.bashrc_conda
(base) [juser@proteusi01]$ conda activate openmm
(openmm) [juser@proteusi01]$ python -m simtk.testInstallation

OpenMM Version: 7.4
Git Revision: b71e92d9ccef7c98cfd6b862eb34095d94fa1b05

There are 2 Platforms available:

1 Reference - Successfully computed forces
2 CPU - Successfully computed forces

Median difference in forces between platforms:

Reference vs. CPU: 6.29225e-06

All differences are within tolerance.
```

In job scripts (request Intel nodes as MKL may or may not work on AMD
nodes):

``` bash
#$ -q all.q
#$ -pe shm 16
#$ -l vendor=intel
#$ -l h_vmem=4G
#$ -l h_rt=12:00:00
. /etc/profile.d/modules.sh
module load shared
module load proteus
module load python/anaconda3

. ~/.bashrc_conda

conda activate openmm
python my_openmm_computation.py
```

### New Queue new.q

On `new.q` (40-core Intel Sky Lake nodes), OpenMM is installed as part
of Anaconda Python, in a separate conda environment named `openmm`.

To use, first create a file named `.bashrc_conda_rh68` in your home
directory:

``` bash
# >>> conda initialize >>>
# !! Contents within this block are managed by 'conda init' !!
__conda_setup="$('/mnt/HA/opt_rh68/anaconda3/bin/conda' 'shell.bash' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__conda_setup"
else
    if [ -f "/mnt/HA/opt_rh68/anaconda3/etc/profile.d/conda.sh" ]; then
        . "/mnt/HA/opt_rh68/anaconda3/etc/profile.d/conda.sh"
    else
        export PATH="/mnt/HA/opt_rh68/anaconda3/bin:$PATH"
    fi
fi
unset __conda_setup
# <<< conda initialize <<<
```

Next, modify your `.bashrc` to handle the case of logging into the
`new.q` nodes -- add the following near the top, *after* the block
mentioning `/etc/bashrc`:

``` bash
if [ -n "${PROTEUSFLAVOR}" ]
then
    echo "PROTEUSFLAVOR = ${PROTEUSFLAVOR}"
    module load proteus
    if [ "${PROTEUSFLAVOR}" == "GPU_RH68" ]
    then
        module load proteus-gpu
    elif [ "${PROTEUSFLAVOR}" == "RH68" ]
    then
        module load proteus-rh68
        . ~/.bashrc_conda_rh68
    fi
fi
```

To use OpenMM interactively, activate the [conda environment](/Anaconda#Conda_Environments "wikilink"). First, request an
interactive session in `new.q`:

``` text
[juser@proteusia01]$ qlogin -q new.q -pe shm 40 -l h_rt=4:00:00,h_vmem=3G
...
(base) [juser@ic23n02]$ module load python/anaconda3
(base) [juser@ic23n02]$ conda activate openmm
(openmm) [juser@ic23n02]$ python -m simtk.testInstallation

OpenMM Version: 7.4
Git Revision: b71e92d9ccef7c98cfd6b862eb34095d94fa1b05

There are 2 Platforms available:

1 Reference - Successfully computed forces
2 CPU - Successfully computed forces

Median difference in forces between platforms:

Reference vs. CPU: 6.29459e-06

All differences are within tolerance.
```

When writing job scripts:

``` bash
#$ -q new.q
#$ -pe shm 40
...
. /etc/profile.d/modules.sh
module load shared
module load proteus-rh68
module load python/anaconda3

. ~/.bashrc_conda_rh68

conda activate openmm
python my_openmm_computation.py
```

See Also
--------

-   [Compiling OpenMM](/Compiling_OpenMM "wikilink")

References
----------

<references/>

[1] [OpenMM web site](http://openmm.org/)

[2] [OpenMMTools](https://openmmtools.readthedocs.io/en/0.18.1/index.html)