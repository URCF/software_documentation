---
title: Singularity
permalink: /Singularity/
---

**Singularity**[1][2][3] containers can be run on Picotte, only on
compute nodes (standard, GPU, and big memory). Singularity can run
Docker images/containers.

Installed version: use the command

-   "`singularity --version`" or
-   "`apptainer --version`"

Containers
----------

Containerization of (enterprise) workloads and in software development
has gained wide adoption, especially using Docker.[4][5] Containers use
operating system-level virtualization to provide isoloated environments,
while sharing the same operating system kernel.[6] Because containers
share the same kernel, they consume fewer resources than virtual
machines like VirtualBox or VMware.

Docker, however, is not suitable for high performance computing (HPC)
applications due to security considerations. (Specifically, running
Docker requires administrative access.) There may also be issues with
access to underlying high performance hardware typical in an HPC
environment, like InfiniBand and OmniPath network devices, and GPUs.

For HPC environments, Singularity is well-suited. Singularity was
created at Lawrence Berkeley National Laboratory specifically for
application in HPC environments. Singularity has forked into
Singularity, with commercial enhancements by
[Sylabs.io](https://sylabs.io), and Apptainer, a purely open source
version, part of the Linux Foundation. (See below.)

### Docker vs. Singularity

-   Docker and Singularity have their own container formats.
-   Docker containers may be imported to run via Singularity.
-   Docker containers need root (i.e. administrative) privileges for
    full functionality which is not suitable for a shared HPC
    environment, while Singularity allows working with containers as a
    regular user.

### Apptainer

As of November 2021, the open source Singularity project has been
renamed Apptainer[7] and moved into the Linux Foundation. This is to
ensure its continued development, and partly to distinguish it from the
commercial Singularity product from Sylabs.[8] This section of the
annoucement clarifies how continued development of Apptainer and Sylabs
Singularity will proceed:

> <b>Will there be continual alignment between Sylabs' Singularity and
> Apptainer?</b>
> Initially, we anticipate yes, but over time we anticipate that the
> paths of the projects will diverge as both projects continue to
> mature. For Apptainer, there is significant interest in better
> alignment with LF and CNCF capabilities like Sigstore, ORAS, and
> CI/CD.

Sylabs maintains an open source version of Singularity, called
[“Singularity CE.”](https://github.com/sylabs/singularity)

As of Feb 20, 2023, Picotte has migrated to using Apptainer.

Running Singularity
-------------------

For the example here, we will download and run the lolcow image which
contains the `cowsay` program.

### Interactively

Outline:

-   Start an interactive session
-   Pull an image
-   Run it

#### Start an Interactive Session

``` text
[juser@node042 ~]$ srun --mem=4G --ntasks=1 --time=1:00:00 --pty /bin/bash
```

#### Pull an Image

We will pull the latest [lolcow image from Singularity Library](https://cloud.sylabs.io/library/_container/5b9e91c694feb900016ea40b).

``` text
[juser@node042 ~]$ singularity pull --arch amd64 library://sylabsed/examples/lolcow:latest
[juser@node042 ~]$ ls -l
total 98672
-rwxrwxr-x 1 juser someGrp 83791872 Mar  4 00:41 lolcow_latest.sif
```

You can also pull a Docker image to a Singularity image:

``` text
[juser@node042 ~]$ singularity pull lolcow_latest.sif docker://godlovedc/lolcow
```

#### Run the Image

First, we look at the file `/etc/os-release` in the container, to see
what version of Linux it is. (Note that it is Ubuntu, even though all
Picotte nodes run Red Hat.) Then, we run the `cowsay` program. The image
itself is executable, with a default action called the "runscript". Or,
you can do "`singularity exec`" to specify commandline arguments.

``` text
[juser@node042 ~]$ ./lolcow_latest.sif
 ______________________________________
/ You like to form new friendships and \
\ make new acquaintances.              /
 --------------------------------------
        \   ^__^
         \  (oo)_______
            (__)\       )\/\
                ||----w |
                ||     ||
[juser@node042 ~]$ singularity exec lolcow_latest.sif cat /etc/os-release
NAME="Ubuntu"
VERSION="18.10 (Cosmic Cuttlefish)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu Cosmic Cuttlefish (development branch)"
VERSION_ID="18.10"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
VERSION_CODENAME=cosmic
UBUNTU_CODENAME=cosmic
[juser@node042 ~]$ singularity exec lolcow_latest.sif cowsay Picotte is awesome
 ____________________
< Picotte is awesome >
 --------------------
        \   ^__^
         \  (oo)_______
            (__)\       )\/\
                ||----w |
                ||     ||
```

N.B. You can actually run the image without an explicit pull:

``` text
[juser@node042 ~]$ singularity run docker://godlovedc/lolcow
INFO:    Using cached SIF image
 _____________________________________
/ Your talents will be recognized and \
\ suitably rewarded.                  /
 -------------------------------------
        \   ^__^
         \  (oo)_______
            (__)\       )\/\
                ||----w |
                ||     ||
```

The first “`singularity run`” will pull the image, and create a SIF file
in your personal cache, which will be used for subsequent
“`singularity run`” commands.

### Batch Mode

Outline:

-   Have an image already downloaded and configured
-   Submit a batch job to execute the image

#### Prepare Image

In the step above, we already have the image file downloaded:
`lolcow_latest.sif`

#### Create and Submit Batch Job

The job script will just run the `singularity exec` line:

``` bash
#!/bin/bash
#SBATCH -J singularity_test
#SBATCH -o singularity_test.out
#SBATCH -e singularity_test.err
#SBATCH -p def
#SBATCH -t 0-00:30
#SBATCH -N 1
#SBATCH -c 1
#SBATCH --mem=1000

singularity exec lolcow_latest.sif cowsay Picotte batch jobs are great
```

Then submit, and the output will be in `singularity_test.out`.

``` text
[juser@picotte001 ~]$ cat singularity_test.out
 ______________________________
< Picotte batch jobs are great >
 ------------------------------
        \   ^__^
         \  (oo)_______
            (__)\       )\/\
                ||----w |
                ||     ||
```

### Binding Directories

Think of the Singularity container (or Docker container, for that
matter) as a virtual computer running on the physical node (called the
“host”).

The Singularity container has no knowledge of the host filesystem. For
the application in the container to access files in the host, a “bind”
must be specified, which maps a directory in the host to a directory in
the container.

For example:[9]

``` bash
    singularity exec --home $BEEGFS_TMPDIR:/home --bind $TMP:/tmp --nv $TF_IMG python /home/tensorflow/classify_flowers.py
```

The above binds:

-   the host directory given by the environment variable
    `$BEEGFS_TMPDIR` to the `/home` in the container
-   the host directory given by the environment variable `$TMP` (i.e.
    job-specific temporary directory) to the `/tmp` directory in the
    container

### Inspecting the Singularity Image

You can inspect the Singularity image (output in JSON):

``` text
[juser@picotte001 ~]$ singularity inspect --all lolcow_latest.sif
{
        "data": {
                "attributes": {
                        "environment": {
                                "/.singularity.d/env/10-docker2singularity.sh": "#!/bin/sh\nexport PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                                "/.singularity.d/env/90-environment.sh": "#!/bin/sh\n#Custom environment shell code should follow\n\n\n    export LC_ALL=C\n    export PATH=/usr/games:$PATH"
                        },
                        "labels": {
                                "org.label-schema.build-date": "Tuesday_5_March_2019_7:55:21_-05",
                                "org.label-schema.schema-version": "1.0",
                                "org.label-schema.usage.singularity.deffile.bootstrap": "library",
                                "org.label-schema.usage.singularity.deffile.from": "ubuntu:latest",
                                "org.label-schema.usage.singularity.version": "3.1.0-rc4"
                        },
                        "runscript": "#!/bin/sh\n\n    fortune | cowsay | lolcat",
                        "deffile": "BootStrap: library\nFrom: ubuntu:latest\n\n%post\n    apt-get -y update\n    apt-get -y install fortune cowsay lolcat\n\n%environment\n    export LC_ALL=C\n    export PATH=/usr/games:$PATH\n\n%runscript\n    fortune | cowsay | lolcat\n",
                        "startscript": "#!/bin/sh"
                }
        },
        "type": "container"
}
```

Documentation
-------------

Interactive help:

``` text
[juser@gpu007 ~]$ singularity help
Linux container platform optimized for High Performance Computing (HPC) and
Enterprise Performance Computing (EPC)

Usage:
  singularity [global options...]
...
Examples:
  $ singularity help <command> [<subcommand>]
  $ singularity help build
  $ singularity help instance start

For additional help or support, please visit https://singularity.hpcng.org/help/
```

N.B. the given URL does not work.

Documentation is also available on the web.[10]

Using GPUs in Singularity Containers (with Docker image)
--------------------------------------------------------

To run Singularity to have access to the GPU devices, use the "`--nv`"
option to `singularity exec/run`. Here we reproduce the example in the
Singularity User Guide.[11] Images bundling TensorFlow tend to be large,
so it is best to do a "`singularity pull`" before running. And it is
better to do it in the group directory rather than your home directory,
as home directories are subject to a limit (quota).

#### Download Image

We will download the [Docker image `tensorflow/tensorflow:latest-gpu`](https://hub.docker.com/r/tensorflow/tensorflow).

``` text
[juser@picotte001 ~]$ srun -p gpu --gres=gpu:1 --cpus-per-gpu=4 --mem=16G --time=1:00:00 --pty /bin/bash
[juser@gpu001 ~]$ singularity pull docker://tensorflow/tensorflow:latest-gpu
INFO:    Converting OCI blobs to SIF format
WARNING: 'nodev' mount option set on /local, it could be a source of failure during build process
INFO:    Starting build...
Getting image source signatures
Copying blob 171857c49d0f done
...
Writing manifest to image destination
Storing signatures
2021/03/04 01:05:40  info unpack layer: sha256:171857c49d0f5e2ebf623e6cb36a8bcad585ed0c2aa99c87a055df034c1e5848
...
INFO:    Creating SIF file...
```

#### Execute Image

We will run Python and use Tensorflow to query the available GPU
devices. Since we requested "`--gres=gpu:1`", we expect to see one GPU
device. We can also run `nvidia-smi` to query the GPU devices.

``` text
[juser@gpu001 ~]$ singularity run --nv tensorflow_latest-gpu.sif

________                               _______________
___  __/__________________________________  ____/__  /________      __
__  /  _  _ _  __ _  ___/  __ _  ___/_  /_   __  /_  __ _ | /| / /
_  /   /  __/  / / /(__  )/ /_/ /  /   _  __/   _  / / /_/ /_ |/ |/ /
/_/    ___//_/ /_//____/ ____//_/    /_/      /_/  ____/____/|__/


You are running this container as user with ID 1234 and group 1234,
which should map to the ID and group for your user on the Docker host. Great!

Singularity> python
Python 3.6.9 (default, Oct  8 2020, 12:12:24)
[GCC 8.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> from tensorflow.python.client import device_lib
2021-03-04 01:11:40.789737: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcudart.so.11.0
>>> print(device_lib.list_local_devices())
2021-03-04 01:11:51.058824: I tensorflow/core/platform/cpu_feature_guard.cc:142] This TensorFlow binary is optimized with oneAPI Deep Neural Network Library (oneDNN) to use the following CPU instructions in performance-critical operations:  AVX2 AVX512F FMA
To enable them in other operations, rebuild TensorFlow with the appropriate compiler flags.
2021-03-04 01:11:51.060213: I tensorflow/compiler/jit/xla_gpu_device.cc:99] Not creating XLA devices, tf_xla_enable_xla_devices not set
2021-03-04 01:11:51.061035: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcuda.so.1
2021-03-04 01:11:51.112241: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1720] Found device 0 with properties:
pciBusID: 0000:18:00.0 name: Tesla V100-SXM2-32GB computeCapability: 7.0
coreClock: 1.53GHz coreCount: 80 deviceMemorySize: 31.75GiB deviceMemoryBandwidth: 836.37GiB/s
2021-03-04 01:11:51.112266: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcudart.so.11.0
2021-03-04 01:11:51.148181: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcublas.so.11
2021-03-04 01:11:51.148216: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcublasLt.so.11
2021-03-04 01:11:51.165382: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcufft.so.10
2021-03-04 01:11:51.177477: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcurand.so.10
2021-03-04 01:11:51.206871: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcusolver.so.10
2021-03-04 01:11:51.216724: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcusparse.so.11
2021-03-04 01:11:51.217906: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcudnn.so.8
2021-03-04 01:11:51.222057: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1862] Adding visible gpu devices: 0
2021-03-04 01:11:51.223571: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcudart.so.11.0
2021-03-04 01:11:53.138048: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1261] Device interconnect StreamExecutor with strength 1 edge matrix:
2021-03-04 01:11:53.138092: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1267]      0
2021-03-04 01:11:53.138102: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1280] 0:   N
2021-03-04 01:11:53.146003: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1406] Created TensorFlow device (/device:GPU:0 with 30128 MB memory) -> physical GPU (device: 0, name: Tesla V100-SXM2-32GB, pci bus id: 0000:18:00.0, compute capability: 7.0)
[name: "/device:CPU:0"
device_type: "CPU"
memory_limit: 268435456
locality {
}
incarnation: 7172651873062288290
, name: "/device:GPU:0"
device_type: "GPU"
memory_limit: 31592420480
locality {
  bus_id: 1
  links {
  }
}
incarnation: 7370608714509062598
physical_device_desc: "device: 0, name: Tesla V100-SXM2-32GB, pci bus id: 0000:18:00.0, compute capability: 7.0"
]
>>> <Ctrl-D>
Singularity>  nvidia-smi
Thu Mar  4 01:13:11 2021
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 450.80.02    Driver Version: 450.80.02    CUDA Version: 11.0     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  Tesla V100-SXM2...  On   | 00000000:18:00.0 Off |                    0 |
| N/A   36C    P0    44W / 300W |      0MiB / 32510MiB |      0%   E. Process |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|  No running processes found                                                 |
+-----------------------------------------------------------------------------+
```

### Batch Mode

Batch mode for GPU-enabled containers works the same as the non-GPU
containers. Just be sure to include the "`--nv`" option. See example
above.

Image Repositories
------------------

-   [Singularity Library](https://cloud.sylabs.io/library)
-   [Docker Hub](https://hub.docker.com)
-   [Nvidia NGC](https://ngc.nvidia.com/catalog/collections)

Building a Singularity Container
--------------------------------

Singularity containers can be built from a *definition file*.[12]

Building a container requires administrative privilege. However,
[Sylabs.io cloud](https://cloud.sylabs.io/builder) offers a service for
building containers. Please sign up for an account there in order to
build containers. Or, contact `urcf-support@drexel.edu` to build a
container.

NOTE You will need to use an interactive session on one of the compute
nodes. Singularity is not available on the login node.

Once you have done so, login to the remote build service, generate an
authentication token and copy it, then use the "`--remote`" option to
`singularity build`:

``` text
[juser@node023 ~]$ singularity remote login
Generate an access token at https://cloud.sylabs.io/auth/tokens, and paste it here.
Token entered will be hidden for security.
Access Token:
INFO:    Access Token Verified!
INFO:    Token stored in /home/juser/.singularity/remote.yaml
[juser@node023 ~]$ singularity build --remote output.sif Singularity.def
```

where `Singularity.def` is the definition file.

### From a Dockerfile

A Dockerfile is a text file that contains all the commands needed to
build a Docker container.[13]

Singularity cannot build a container image directly from a Dockerfile.
The Dockerfile has to be converted to a Singularity definition (a.k.a.
recipe) file, first. The [Singularity Python (spython)](https://singularityhub.github.io/singularity-cli/) package
provides a way to do that.

``` text
[juser@node023 ~]$ module load python/gcc
[juser@node023 ~]$ which spython
/ifs/opt/python/gcc/3.9.1/bin/spython
[juser@node023 ~]$ spython recipe Dockerfile > Singularity.def
```

Once the `Singularity.def` file has been created, do a remote build.

If you have trouble building a Singularity container image, please
contact urcf-support@drexel.edu Some images require a significant amount
of resources, which prevent them from being built on the unpaid tier of
the Singularity remote build system.

Managing the Singularity Image Cache
------------------------------------

When you build a Singularity image, Singularity stores images and
"blobs" (Binary Large OBjects) in a cache directory. On Picotte, every
user will have their own cache directory on the BeeGFS filesystem in:

`    /beegfs/SingularityCache/${USER}`

### View cache contents

You can view the contents of the cache:

``` text
[juser@gpu007 ~]$ singularity cache list -v
NAME                     DATE CREATED           SIZE             TYPE
09b7747c19d1904faeb004   2022-02-15 16:58:42    359.77 KiB       blob
195e3ae4368f9dc07fef67   2022-02-15 16:58:56    6.09 KiB         blob
297f2c1afa6cf9ac9c1712   2022-02-15 17:01:43    2.98 KiB         blob
2c9cef21f753494c2a3353   2022-02-15 16:58:55    1.18 MiB         blob
...
f52357ed8777c8292ccb37   2022-02-15 16:58:40    26.75 MiB        blob
fd8536d9c6a360ac662893   2022-02-15 17:01:39    3.91 MiB         blob
febbb989eee6ef1ff8701f   2022-02-15 16:58:42    0.13 KiB         blob
aa095dcdb175e10132a586   2022-02-15 17:01:59    262.99 MiB       oci-tmp
c6cf2371ad7eb491925e58   2022-02-15 16:59:23    382.47 MiB       oci-tmp

There are 2 container file(s) using 645.46 MiB and 51 oci blob file(s) using 749.59 MiB of space
Total space used: 1.36 GiB
```

### Delete cache contents

You can delete cache contents using the following command. To retain the
contents, and do a "dry run" without actually deleting anything, use the
"`--dry-run`" option.

``` text
[juser@gpu007 ~]$ singularity cache clean
This will delete everything in your cache (containers from all sources and OCI blobs).
Hint: You can see exactly what would be deleted by canceling and using the --dry-run option.
Do you want to continue? [N/y] y
INFO:    Removing blob cache entry: blobs
INFO:    Removing blob cache entry: index.json
INFO:    Removing blob cache entry: oci-layout
INFO:    No cached files to remove at /beegfs/SingularityCache/dwc62/cache/library
INFO:    Removing oci-tmp cache entry: aa095dcdb175e10132a5862204bf91e6f374f72ba9f2360d9ff5c45ae67785fd
INFO:    Removing oci-tmp cache entry: c6cf2371ad7eb491925e58cc28ee0ef9f0bd84a15634bb0e7f818c7d07e8877e
INFO:    No cached files to remove at /beegfs/SingularityCache/dwc62/cache/shub
INFO:    No cached files to remove at /beegfs/SingularityCache/dwc62/cache/oras
INFO:    No cached files to remove at /beegfs/SingularityCache/dwc62/cache/net
```

See Also
--------

-   [Apptainer Documentation](https://apptainer.org/docs/)
-   [Harvard University Faculty of Arts & Sciences Research Computing Knowledge Base - Singularity on the Cluster](https://docs.rc.fas.harvard.edu/kb/singularity-on-the-cluster/)
-   [Apptainer Quick Start Guide](https://apptainer.org/docs/user/main/quick_start.html)
-   [Slurm - Job Script Example 05 TensorFlow Singularity](/Slurm_-_Job_Script_Example_05_TensorFlow_Singularity "wikilink")
-   [Docker](/Docker "wikilink")

References
----------

<references/>

[1]

[2] [Sylabs.io - Singularity](https://sylabs.io/singularity)

[3] [:wikipedia:Singularity (software)](/:wikipedia:Singularity_(software) "wikilink")

[4] [Docker website](https://www.docker.com/)

[5] [:wikipedia:Docker (software)](/:wikipedia:Docker_(software) "wikilink")

[6] [:wikipedia:Kernel (operating system)](/:wikipedia:Kernel_(operating_system) "wikilink")

[7] [Apptainer website](https://apptainer.org)

[8] [Apptainer - Community Announcement 2021-11-30](https://apptainer.org/news/community-announcement-20211130)

[9] [Slurm - Job Script Example 05 TensorFlow Singularity](/Slurm_-_Job_Script_Example_05_TensorFlow_Singularity "wikilink")

[10] [SingularityCE 3.8 User Guide](https://sylabs.io/guides/3.8/user-guide/)

[11] [Singularity 3.8 User Guide - GPU Support](https://sylabs.io/guides/3.8/user-guide/gpu.html)

[12] [Singularity User Guide: Build a Container](https://sylabs.io/guides/3.8/user-guide/build_a_container.html)

[13] [Docker Reference: Dockerfile Reference](https://docs.docker.com/engine/reference/builder/)