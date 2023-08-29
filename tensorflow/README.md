---
title: TensorFlow
permalink: /TensorFlow/
---

**TensorFlow** is an open source software library for numerical
computation using data flow graphs, typically used in machine
learning.[1]

Picotte
=======

Due to the way Anaconda requires modifications to the user's login
script, once a user starts utilizing Anaconda Python, it will be
complicated to move to a non-Anaconda version, or to use your own copy
of Anaconda.

### Running TensorFLow

There are 4 ways to use TensorFlow:

1.  using Anaconda Python. You can either install a private version of
    Anaconda, or use the version URCF provides. We recommend installing
    a private version.
2.  using Singularity
3.  using a virtualenv that you create yourself. This is the method
    recommended in the TensorFlow documentation.
4.  one of the other provided Python modules, and installing a private
    version of TensorFlow using `python3 -m pip install --user`.[2]

Installing Anaconda for Yourself
--------------------------------

See: [Anaconda](/Anaconda "wikilink")

N.B. This is **not recommended** because mismatches between some of the
CUDA libraries provided by Anaconda and the CUDA drivers installed on
Picotte GPU nodes may cause job scripts to crash, and even cause nodes
to crash.

### Using TensorFlow in Anaconda

See: [Slurm - Job Script Example 05a TensorFlow With Anaconda Python](/Slurm_-_Job_Script_Example_05a_TensorFlow_With_Anaconda_Python "wikilink")

Using Anaconda Python installed by URCF:

-   First, load the modulefile

`    python/anaconda3`

-   Then, initialize your login scripts by doing:
    `conda init _name_of_shell_` ("_name_of_shell_" should be "bash"
    for most users)
-   And, log out then log back in

There are multiple Conda environments[3] providing TensorFlow. Please
use the command:

`    conda env list`

to see a list.

To install a different version, specify the version number:

``` text
(base) [juser@picotte001 ~]$ conda create -n tf21-gpu tensorflow-gpu=2.1.0
```

Suggestion: Use environment names which are different from existing
ones. Do "`conda env list`" to see a list.

Using TensorFlow via a Singularity Container
--------------------------------------------

See: [Slurm - Job Script Example 05 TensorFlow Singularity](/Slurm_-_Job_Script_Example_05_TensorFlow_Singularity "wikilink")
and [Singularity](/Singularity "wikilink")

-   Check if the image you need is already on Picotte in:
    `/beegfs/SingularityImages/`
-   TensorFlow's official Docker images are on DockerHub:
    <https://hub.docker.com/r/tensorflow/tensorflow/>
-   TensorFlow is also available via the NVIDIA GPU Cloud Singularity
    containers. See: [NVIDIA GPU Cloud Containers](/NVIDIA_GPU_Cloud_Containers "wikilink")

Installing a Private TensorFlow for Your Research Group with venv
-----------------------------------------------------------------

This is the method that was previously recommended in the TensorFlow
documentation.[4] You can use one of the available Python modulefiles,
and install a private TensorFlow there using a virtual env and pip.

### Using Python 3.10 to install the latest TensorFlow

-   Broken out into separate articles due to the frequency of TensorFlow
    updates.
    -   [Installing TensorFlow 2.11.0 using pip and venv](/Installing_TensorFlow_2.11.0_using_pip_and_venv "wikilink")
    -   [Installing TensorFlow 2.10.1 using pip and venv](/Installing_TensorFlow_2.10.1_using_pip_and_venv "wikilink")
    -   [Installing TensorFlow 2.9.3 using pip and venv](/Installing_TensorFlow_2.9.3_using_pip_and_venv "wikilink")

Installing Private TensorFlow with pip
--------------------------------------

Using a venv, as detailed in the previous section, is recommended over
this method.

-   Load your desired version of Python, e.g.
    "`module load python/gcc/3.10`"
-   Use pip to install a private copy of TensorFlow:
    "`pip3 install --user tensorflow-gpu tensorflow-datasets`"
    -   This installs in your home directory:
        `~/.local/lib/python3.10/site-packages`

### Using Private TensorFlow

Just load the Python you used:

`    module load python/gcc/3.10`

And TF will be directly available.

Sample TensorFlow Datasets
--------------------------

-   See: [Sample TensorFlow Datasets](/Sample_TensorFlow_Datasets "wikilink")

Proteus
=======

PROTEUS HAS BEEN DECOMMISSIONED.

Installed Versions
------------------

### CPU-only Version

TensorFlow 1.3.0 is available in the [Intel Distribution for Python](/Python#Intel-Optimized_Versions "wikilink"). Load the
modulefile:

`   python/intelpython/3.5.3`

The TensorFlow in python/intelpython/3.6.x does not work due to a
library dependency.

This is optimized to run on the Intel nodes, and may be sub-optimal on
the AMD nodes.

### GPU Version

TensorFlow 1.8.0 using NVIDIA GPUs is available. This uses Anaconda
Python 3. This is provided by the Anaconda "`tensorflow-gpu`" package.

Use the following modulefile from `/mnt/HA/opt_cuda90/modulefiles` (on
the GPU nodes, this should be the first modulefiles directory searched):

`    python/anaconda3`

And then activate the `tensorflow` conda environment:

`    conda activate tensorflow`

To make sure you have the right version of Python in the right
environment:

`    [juser@gpu01 ~]$ which python3`
`    /mnt/HA/opt_cuda90/anaconda3/envs/tensorflow/bin/python3`

Once you are done, deactivate the environment:

`    conda deactivate`

**NOTE**

-   This TensorFlow installation uses Anaconda Python 3.6. Do not load
    any other version of Python using modulefiles.
-   General information about running jobs on GPU nodes is here: [GPU Jobs](/GPU_Jobs "wikilink")
    -   In particular, h_vmem requests are irregular
-   See the official documentation for information on using the GPUs and
    using multiple GPUs: <https://www.tensorflow.org/guide/gpu>
-   [Keras](/Keras "wikilink") 2.2.0 is also available under this Conda
    environment.

Distributed TensorFlow
----------------------

TensorFlow can run on **only one node (server) at a time**, though it
may use multiple GPU devices on that node.

While there is a distributed feature in TensorFlow,[5] that is not
currently tested on Picotte.

Example Job Script
------------------

Here are two examples for the GPU-enabled TensorFlow:

-   [Slurm - Job Script Example 08 TensorFlow using virtualenv](/Slurm_-_Job_Script_Example_08_TensorFlow_using_virtualenv "wikilink")
-   [Slurm - Job Script Example 08a TensorFlow multi-GPU using virtualenv](/Slurm_-_Job_Script_Example_08a_TensorFlow_multi-GPU_using_virtualenv "wikilink")

See Also
--------

-   [Compiling TensorFlow](/Compiling_TensorFlow "wikilink")

References
----------

<references/>

[1] [TensorFlow web site](https://www.tensorflow.org/)

[2] [TensorFlow Documentation - Install TensorFlow with pip](https://www.tensorflow.org/install/pip)

[3] [Conda User Guide: Conda Environments](https://conda.io/projects/conda/en/latest/user-guide/concepts/environments.html)

[4]

[5] [TensorFlow Guide - Accelerators - Distributed Training](https://www.tensorflow.org/guide/distributed_training)
