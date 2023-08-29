---
title: NVIDIA CUDA on Proteus
permalink: /NVIDIA_CUDA_on_Proteus/
---

Hardware
--------

-   Eight nodes (gpu01 -- gpu08), each with
-   dual NVIDIA K20Xm - Kepler architecture, Tesla microarchitecture,
    GK110 die[1][2] -- 2688 CUDA cores, 6144 MB RAM

Installed Version
-----------------

NVIDIA CUDA 9.0 is installed on all GPU nodes.

### Other NVIDIA Libraries

-   NCCL - located in /usr/local/cuda/nccl

Compile Options
---------------

### Architecture

Compile for native compute capability target 3.5 architecture
(`sm_35`).[3]

`   -gencode arch=compute_35,code=sm_35`

If the software expects a `CUDA_ARCH` environment variable, use:

`   CUDA_ARCH=35`

### C Compiler

CUDA will not work with the Intel Compiler. Please use GCC: the
modulefile gcc/4.8.1 that is loaded by default will work.

Hybrid MPI with CUDA
--------------------

See <https://www.ccv.brown.edu/doc/mixing-mpi-and-cuda.html> for an
example.

**NOTES**

1.  This may or may not improve the performance of your code.
    Benchmarking your own code is necessary.
2.  Simply using mpicc to compile a CUDA-enabled code is not enough to
    generate an executable that does both CUDA and MPI. The code has to
    be specifically written to integrate CUDA with MPI.

Running CUDA Jobs
-----------------

See: [GPU Jobs on Proteus](/GPU_Jobs_on_Proteus "wikilink")

PyCUDA
------

**PyCUDA**[4][5] is a Python interface to CUDA.

### To Use

PyCUDA is installed on the GPU nodes as part of the **python37** conda
environment.

-   First, load the `python/anaconda3` module:
    `module load python/anaconda3`
-   Then, activate the `python37` environment: `conda activate python37`

References
----------

<references/>

[1] [NVIDIA Tesla K-Series Overview (PDF)](http://www.nvidia.com/content/tesla/pdf/Tesla-KSeries-Overview-LR.pdf)

[2] [TechPowerUp Hardware Database: NVIDIA K20Xm](https://www.techpowerup.com/gpudb/1884/tesla-k20xm)

[3] [NVIDIA CUDA Toolkit 6.0 Documentation - Kepler Tuning Guide](http://docs.nvidia.com/cuda/kepler-tuning-guide/#application-compatibility)

[4] [PyCUDA webpage at NVIDIA](https://developer.nvidia.com/pycuda)

[5] [PyCUDA webpage](https://mathema.tician.de/software/pycuda/)