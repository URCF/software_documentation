---
title: FEniCS
permalink: /FEniCS/
---

The **FEniCS** Project is an open-source platform for solving partial
differential equations.[1]

Installed Versions
------------------

FEniCS **2019.1** binary distribution via Anaconda is installed on
Proteus. It is installed as a conda environment in the
**`python/anaconda3`** installation. This is only on the `new.q` Skylake
nodes (`ic23`).

For interactive use, request a session on one of the `new.q` nodes:

`   qlogin -q new.q -pe shm 40 -l h_vmem=4G -l h_rt=48:00:00`

Then, load the modules:

`   python/anaconda3`
`   gcc/7.4.0`

FEniCS uses a just-in-time (JIT) compiler, which requires a C++ compiler
supporting the C++11 standard. The system g++ 4.4, and older Proteus g++
4.8 do not support this standard. The `gcc/7.4.0` module provides a
suitable C++ compiler.

Next, set up your shell to use conda, and activate the environment:

`   [juser@ic23n02 ~]$ source $ANACONDAHOME/etc/profile.d/conda.sh`
`   [juser@ic23n02 ~]$ eval "$(conda shell.bash hook)"`
`   [juser@ic23n02 ~]$ conda activate fenicsproject`

You should see your prompt change to indicate the new environment:

`   (fenicsproject) [juser@ic23n02 ~]$`

**NOTE** since this is a pre-compiled binary distribution, it may not
provide the best performance possible.

References
----------

<references/>

[1] [FEniCS Project website](https://fenicsproject.org)