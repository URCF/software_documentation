---
title: GPU Jobs on Picotte
permalink: /GPU_Jobs_on_Picotte/
---

Summary
-------

CUDA toolkit to use -- one of:

-   11.0
-   11.1
-   11.2
-   11.4

Options for sbatch:

-   Partition: `--partition=gpu`
-   ~~GRES (Generic RESources), where 'N' is the number of GPU devices
    per node to use (&lt;= 4): --gres=gpu:N~~
-   Number of GPUs per node: `--gpus-per-node=4`
    -   this is equivalent to `--gres=gpu:4`, which is still a valid way
        of specifying GPUs
-   CAUTION sometimes "--cpus-per-gpu" behaves as expected, sometimes
    "--ntasks" needs to be specified; we have not pinned down when one
    works and when it does not; please check your application.
    -   CPU cores: `--cpus-per-gpu=12` (i.e. allocates 12 CPU cores per
        allocated GPU device)
    -   Manually specify the total number of slots/tasks, taking into
        account number of nodes as well: use 12 per GPU, e.g.
        `--nodes=2 --gpus-per-node=4 --ntasks=96`
-   Memory can be specified per GPU: `--mem-per-gpu=42G`

**MISSING TMPDIR /local/scratch/_jobid_** -- this happens when
specifying "--ntasks", but not when specifying "--cpus-per-gpu"

-   If your program emits error messages like
    "`/local/scratch/nnnnnnn: no such file or directory`", manually
    create that directory:

`    mkdir /local/scratch/$SLURM_JOBID`

Module Path
-----------

CUDA-enabled software is in a different directory tree. In your job
scripts, or in interactive sessions, do

`    module use /ifs/opt_cuda/modulefiles`

NVIDIA GPU Cloud (NGC) Containers
---------------------------------

See: [NVIDIA GPU Cloud
Containers](/NVIDIA_GPU_Cloud_Containers "wikilink")

Do:

`    module use /ifs/opt_cuda/ngc-container-environment-modules`

Hybrid MPI with CUDA
--------------------

Modulefile (in `/ifs/opt_cuda/modulefiles`)

-   picotte-openmpi/cuda11.0/4.1.0 (Uses GCC)
-   picotte-openmpi/cuda11.2/4.1.0 (Uses Intel ICC)

See Also
--------

-   [Writing Slurm Job Scripts](/Writing_Slurm_Job_Scripts "wikilink")
-   [NVIDIA CUDA on Picotte](/NVIDIA_CUDA_on_Picotte "wikilink")

