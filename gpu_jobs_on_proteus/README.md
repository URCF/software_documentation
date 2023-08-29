---
title: GPU Jobs on Proteus
permalink: /GPU_Jobs_on_Proteus/
---

Jobs using NVIDIA CUDA to run on GPUs have special requirements due to
the GPU nodes running a newer version of Red Hat Enterprise Linux
(needed to support a recent version of NVIDIA CUDA.)

CUDA Version 9.0 Only
---------------------

Only CUDA 9.0 is supported on Proteus. Constraints on kernel version,
hardware, and existing software mean that it is not feasible to use a
later version of CUDA.

No special modulefiles need to be loaded to use CUDA, unlike the
previous system where various "cuda60" modulefiles had to be loaded.
CUDA 9.0 is installed as part of the system defaults. However, one
modulefile providing CUDA-enabled software (e.g. NAMD) should be loaded.
See below.

Official documentation is available online.[1]

### Environment Setup

Your `.bashrc` should have this section near the top:

``` bash
if [ -n "${PROTEUSFLAVOR}" ]; then
    module load proteus
    if [ "${PROTEUSFLAVOR}" == "GPU_RH68" ]; then
        module load proteus-rh68
        module load proteus-gpu
        module unload gcc

        __conda_setup="$('/mnt/HA/opt_cuda90/anaconda3/bin/conda' 'shell.bash' 'hook' 2> /dev/null)"
        if [ $? -eq 0 ]; then
            eval "$__conda_setup"
        else
            if [ -f "/mnt/HA/opt_cuda90/anaconda3/etc/profile.d/conda.sh" ]; then
                . "/mnt/HA/opt_cuda90/anaconda3/etc/profile.d/conda.sh"
            else
                export PATH="/mnt/HA/opt_cuda90/anaconda3/bin:$PATH"
            fi
        fi
        unset __conda_setup
    elif [ "${PROTEUSFLAVOR}" == "RH68" ]; then
        module load proteus-rh68
        module unload gcc

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
    elif [ "${PROTEUSFLAVOR}" == "NEW" ]; then
        module load proteus-new
        module unload /opt/mellanox/hpcx/modulefiles/hpcx-stack
        module unload gcc
    fi
fi
```

GPU Hardware
------------

For information about the GPU hardware, and the nodes where the GPU
devices are installed, see: [GPU Nodes](/GPU_Nodes "wikilink")

Interactive Use
---------------

-   Request an interactive session as a job using "`qlogin`"

### Miscellaneous Issues

-   If the `less` file pager misbehaves, do the following (or the
    equivalent in your shell):

`    export -n LESSOPEN`

Limits
------

Because there are only a few GPU machines, jobs are limited to:

-   96 hours h_rt
-   64 slots (i.e. 4 nodes = 8 GPU devices)

This is subject to change without notice.

Queue
-----

Submit to the **gpu.q**

`   #$ -q gpu.q`

Resource Requests
-----------------

### Requesting GPU Resources

The GPUs are defined as a resource map, so individual GPUs may be
requested. The request is made **per node**. **ALL** GPU nodes in
Proteus have **2** (two) GPU devices only. I.e. the "gpu" resource
cannot be greater than 2.

Examples:

`    ### serial (i.e. 1 slot), 1 GPU`
`    #$ -l gpu=1`

`    ### serial (i.e. 1 slot), 2 GPUs`
`    #$ -l gpu=2`

`    ### 16 slots (1 node), 2 GPUs`
`    #$ -pe shm 16`
`    #$ -l gpu=2`

`    ### 32 slots (2 nodes), 2 GPUs per node (= 4 GPUs)`
`    #$ -pe fixed16 32`
`    #$ -l gpu=2`

`    ### DO NOT DO THIS because this does NOT use as compact a distribution of GPUs as possible`
`    ### 48 slots (3 nodes), 1 GPU per node (= 3 GPUs)`
`    #$ -pe fixed16 48`
`    #$ -l gpu=1`

`    ### 128 slots (= 8 nodes), 2 GPUs per node (= 16 GPUs)`
`    #$ -pe fixed16 128`
`    #$ -l gpu=2`

**NOTE** Some GPU-enabled programs do all the parallelism only on the
GPUs, running only a single-threaded process on the CPU as a
"controller". Which means you would not need to request a PE for these
types of programs.

Once the job is running, "qstat -j JOBID" will show the GPU device
mapping:

`    resource map          1:    ngpus=gpu05.cm.cluster=(GPU0 GPU1), ngpus=gpu02.cm.cluster=(GPU0 GPU1)`

### Memory Resources

Due to the way Univa Grid Engine measures memory usage on CPU+GPU, the
`h_vmem` memory limit needs to be increased to what looks like an
impossible amount.

For example, here is a [GROMACS](/GROMACS "wikilink") job that runs on a
single node using both GPUs:

``` bash
#$ -pe fixed16 16
#$ -q gpu.q
#$ -l excl
#$ -l gpu=2
#$ -l h_rt=24:00:00
#$ -l m_mem_free=3G
#$ -l h_vmem=100G

...

$MPI_RUN $GMXBIN/gmx_mpi mdrun -deffnm ethanol.0 -dhdl ethanol.0.dhdl.xvg
```

On the node where the job is running, the "free(1)" command shows memory
usage of 4.3 GB:

``` text
             total       used       free     shared    buffers     cached
Mem:           62G       4.3G        58G        24M       642M       1.7G
-/+ buffers/cache:       1.9G        61G
Swap:          11G         0B        11G
```

However, "`qstat -j `*`JOBID`*" reports a total memory usage (`maxvmem`)
of 401.653 GB:

``` text
usage                 1:    wallclock=06:25:31, cpu=01:29:50, mem=540631.43057 GBs, io=0.02193 GB, iow=7.750 s, ioops=23392, vmem=401.653G, maxvmem=401.653G, rss=527.246M, pss=477.890M, smem=64.695M, pmem=462.551M, maxrss=527.246M, maxpss=478.336M
```

So, GPU jobs should set a very large `h_vmem` threshold. Typically,
about 100G per node should work.

Environment
-----------

The GPU nodes have a different configuration from the rest of the
compute nodes in Proteus. Unfortunately, automating the environment
module loading does not seem to work properly.

Here are the changes to be made.

### Add to ~/.bashrc

Add the following near the top of your `.bashrc` file:

``` bash
module load proteus
if [ ${HOSTNAME:0:3} == "gpu" ]
then
    module load proteus-gpu
fi

### add more "module load ..." commands below
module load git
module load somethingelse
```

If you use a different default shell, you will need to translate the
above.

### Interactive Jobs

The above `.bashrc` should take care of the interactive environment. You
can check by doing:

`    `**`[juser@gpu01`` ``~]$`**` module avail git`
`    `
`    ------------------------------------ /mnt/HA/opt_rh68/modulefiles -------------------------------------`
`    git/2.18.0`
`    `
`    --------------------------------------- /mnt/HA/opt/modulefiles ---------------------------------------`
`    git/2.12.0 git/2.13.0 git/2.16.1 git/2.17.0 git/2.17.1`
`    `
`    `**`[juser@gpu01`` ``~]$`**` module avail namd`
`    ----------------------------------- /mnt/HA/opt_cuda90/modulefiles ------------------------------------`
`    namd/ibverbs-smp-cuda/2.12`
`    `
`    --------------------------------------- /mnt/HA/opt/modulefiles ---------------------------------------`
`    namd/cuda60/openmpi/2.9                      namd/ibverbs-smp-cuda/2.12`
`    namd/gcc/mvapich2/2.9                        namd/intel/2015/cuda/6.0/openmpi/2.10-broken`
`    namd/gcc/openmpi/2.9                         namd/intel/2015/openmpi/2.10`
`    namd/gcc-cuda/openmpi/2.9                    namd/intel-cuda/mvapich2/2.10`
`    namd/ibverbs-smp/2.12                        namd/intel-cuda/mvapich2-gdr/2.10`
`    `
`    `

NOTE:

1.  The path `/mnt/HA/opt_rh68/modulefiles` comes before
    `/mnt/HA/opt/modulefiles`
2.  The path `/mnt/HA/opt_cuda90/modulefiles` provides
    `namd/ibverbs-smp-cuda/2.12`

### Non-interactive Jobs

The first line of your job script, which starts with "`#!`" must be
changed from "`#!/bin/bash`" to

``` bash
#!/bin/bash -l
```

Note the additional option **`-l`**.

You should not need to add the usual modules "shared", "proteus", "gcc",
or "sge/univa". However, adding them does not hurt, say if you are
converting old scripts.

Available CUDA-enabled Applications
-----------------------------------

-   [NAMD\#CUDA-enabled Version using IBVERBS](/NAMD#CUDA-enabled_Version_using_IBVERBS "wikilink") --
    includes an example job script, and calculations for distributing
    MPI ranks/threads
-   [TensorFlow\#GPU Version](/TensorFlow#GPU_Version "wikilink")

Examples:

-   [Job_Script Example 08 TensorFlow MNIST](/Job_Script_Example_08_TensorFlow_MNIST "wikilink")
-   [Job Script Example 09 TensorFlow MNIST Multi-GPU-CNN](/Job_Script_Example_09_TensorFlow_MNIST_Multi-GPU-CNN "wikilink")

Monitoring
----------

GPU load is separate from CPU load, and is not taken into account in the
`np_load` metric reported by `qstat`. GPU load may be monitored via the
Ganglia web interface. E.g. [`gpu0_util` (GPU0 utilization) for the past 2 hours](https://proteusmaster.urcf.drexel.edu/ganglia-proteus/?c=Proteus&m=gpu0_util&r=2hr&s=by%20name&hc=4&mc=2).
You can select the appropriate metric: `gpu0_util`, or `gpu1_util`.

![<File:Aggregated_gpu_utilization.png>](/Aggregated_gpu_utilization.png "wikilink")

![<File:GPU_utilization_all_GPU_nodes.png>](/GPU_utilization_all_GPU_nodes.png "wikilink")

References
----------

<references/>

[1] [NVIDIA CUDA 9.0 Documentation](https://docs.nvidia.com/cuda/archive/9.0/)