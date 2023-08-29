---
title: NVIDIA GPU Cloud Containers
permalink: /NVIDIA_GPU_Cloud_Containers/
---

**NVIDIA GPU Cloud (NGC)** Containers[1] are available on Picotte via
modulefiles, using NGC Container Environment Modules.[2] These
containers use [Singularity](/Singularity "wikilink").

Usage
-----

NGC container environment modules are available only on the GPU nodes.
To use them, do (either in a job script or in an interactive session):

`    module use /ifs/opt_cuda/ngc-container-environment-modules`

To see available modules:

`    module avail`

Available Software
------------------

For a complete list of available software, the [NGC containers web catalog](https://catalog.ngc.nvidia.com/containers) is more convenient
than "module avail".

Software includes:

-   LAMMPS
-   GROMACS
-   M-Star
-   TensorFlow
-   PyTorch

Containers Requiring Login/Authentication
-----------------------------------------

Some containers provided by NGC require login to download and use.

-   For those, you will need to [sign up for an NVIDIA developer account](https://developer.nvidia.com/developer-program).
-   Then, sign in to NGC: <https://ngc.nvidia.com/signin>
-   Once in, you will generate an API key:
    <https://ngc.nvidia.com/setup>
    -   NOTE this API key will be shown only once. Save it in a private
        location when it's shown; e.g. use a password saving app. Or you
        can just generate a new one every time you need one.
-   Then, when you first try to pull a container that requires login, it
    will prompt for a login and password. For the login, enter the
    literal string "`$oauthtoken`" (i.e. without the double-quotes)

``` text
$ singularity pull --docker-login something.sif docker://nvcr.io/nvidia/something:1.0-testing
Enter Docker Username: $oauthtoken
Enter Docker Password: _paste_generated_API_token_here_
```

Example
-------

We will try to run [GROMACS](/GROMACS "wikilink") from an NGC container.

First, start an interactive session on a GPU node (because Singularity
is not available on the login node):

``` text
[juser@picotte001 ~]$ srun -p gpu -G 1 --time=4:00:00 --mem=32G --time=6:00:00 --cpus-per-gpu=12 --pty /bin/bash
[juser@gpu007 ~]$
```

Next, add the NGC modulefiles directory to your module search path:

``` text
[juser@gpu007 ~]$ module use /ifs/opt_cuda/ngc-container-environment-modules
```

Look for a GROMACS package -- note the ones listed under
`/ifs/opt_cuda/ngc-container-environment-modules`:

``` text
[juser@gpu007 ~]$ module avail gromacs
--------------- /ifs/opt_cuda/ngc-container-environment-modules ----------------
   gromacs/2018.2    gromacs/2020.2    gromacs/2021 (D)

----------------------------- /ifs/opt/modulefiles -----------------------------
   gromacs/intel/2020/2021.1    gromacs/intel/2020/2021.3 (D)

-------------------------- /ifs/opt_cuda/modulefiles ---------------------------
   gromacs/cuda11.0/2021.1    gromacs/cuda11.2/2021.3
...
```

Load the "gromacs/2021.1" module from the NGC container environment
modules:

``` text
[juser@gpu007 ~]$ module load gromacs/2020.2
```

The first time you run the GROMACS executable `gmx`, NGC will pull the
container image, and build it. The "SIF file" is the "Singularity Image
File" that is the container. (Output has been truncated here.)

``` text
[juser@gpu007 ~]$ gmx
INFO:    Converting OCI blobs to SIF format
WARNING: 'nodev' mount option set on /local, it could be a source of failure during build process
INFO:    Starting build...
Getting image source signatures
Copying blob 59492a727a6b done
...
Copying config 0ea6d50249 done
Writing manifest to image destination
Storing signatures
2022/02/15 16:05:50  info unpack layer: sha256:59492a727a6b06967eafb9451cacbaddc36be8fb26b7fda7e5a80efe42d7ecb9
...
2022/02/15 16:05:54  info unpack layer: sha256:b4241e4957a347245621b6e39b90a63c070e57e6e8641af8a778d4cbeea027ca
INFO:    Creating SIF file...
INFO Configured container for NVIDIA GPU architecture sm70
           :-) GROMACS - gmx, 2020.2-dev-20200430-5e78835-unknown (-:

                            GROMACS is written by:
...
GROMACS:      gmx, version 2020.2-dev-20200430-5e78835-unknown
Executable:   /usr/local/gromacs//sm70/bin/gmx
Data prefix:  /usr/local/gromacs//sm70
Working dir:  /home/dwc62
Command line:
  gmx
...
```

N.B. The paths listed (`/usr/local/gromacs//sm70/bin/gmx`) are inside
the container, and not on the Picotte GPU node.

### More complex examples

Please see the [NGC Container Environment Modules](https://github.com/NVIDIA/ngc-container-environment-modules)
GitHub README for more complex examples, including a multi-node MPI
computation.

See Also
--------

-   [Singularity](/Singularity "wikilink")
-   [TensorFlow](/TensorFlow "wikilink")
-   [GPU Jobs on Picotte](/GPU_Jobs_on_Picotte "wikilink")

References
----------

<references />

[1] [NGC Container Catalog](https://catalog.ngc.nvidia.com/containers)

[2] [NGC Container Environment Modules](https://github.com/NVIDIA/ngc-container-environment-modules)