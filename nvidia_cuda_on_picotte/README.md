---
title: NVIDIA CUDA on Picotte
permalink: /NVIDIA_CUDA_on_Picotte/
---

Hardware
--------

See: [Picotte Hardware and Software\#GPU compute nodes .2812_nodes.29](/Picotte_Hardware_and_Software#GPU_compute_nodes_.2812_nodes.29 "wikilink")

### Driver

As of December 2021: CUDA Driver 470.57.02 is installed [1]

Installed Versions
------------------

**CUDA 10.1**, **10.2**, **11.0**, **11.1**, **11.2**, **11.4**

**Recommended versions:** CUDA 11.0 (11.0.3), CUDA 11.2 (11.2.2), CUDA
11.4 (11.4.2)

### Modulefiles

"nvhpc" = NVIDIA High Performance Computing Software Development Kit[2]

``` text
cuda11.0/blas
cuda11.0/fft
cuda11.0/nsight
cuda11.0/profiler
cuda11.0/toolkit
cudnn8.0-cuda11.0
nvhpc
nvhpc-byo-compiler
nvhpc-nompi
```

OR

``` text
cuda11.2/blas
cuda11.2/fft
cuda11.2/nsight
cuda11.2/profiler
cuda11.2/toolkit
cudnn8.0-cuda11.2
cudnn8.1-cuda11.2
cutensor-cuda11.2
nvhpc
nvhpc-byo-compiler
nvhpc-nompi
```

OR

``` text
cuda11.4/blas/11.4.2
cuda11.4/fft/11.4.2
cuda11.4/nsight/11.4.2
cuda11.4/profiler/11.4.2
cuda11.4/toolkit/11.4.2
```

### Documentation

-   [Nvidia CUDA Toolkit v11.0.3 Documentation](https://docs.nvidia.com/cuda/archive/11.0/index.html)
-   [Nvidia CUDA Toolkit v11.2.0 Documentation](https://docs.nvidia.com/cuda/archive/11.2.0/index.html)
-   [Nvidia CUDA Toolkit v11.4.2 Documentation](https://docs.nvidia.com/cuda/archive/11.4.2/)

Compile Options
---------------

### Architecture/Compute Capability

Nvidia Tesla V100 (Volta architecture): compute capability
`7.0`[3][4][5]

`   -gencode arch=compute_70,code=sm_70`

and possibly:

`   -arch=sm_70`

If the software expects a `CUDA_ARCH` environment variable, use:

`   CUDA_ARCH=70`

Running CUDA Jobs
-----------------

See: [GPU Jobs on Picotte](/GPU_Jobs_on_Picotte "wikilink")

References
----------

<references/>

[1] [Nvidia Tesla 470.57.02 Release Notes](https://docs.nvidia.com/datacenter/tesla/tesla-release-notes-470-57-02/index.html)

[2] [Nvidia HPC SDK](https://developer.nvidia.com/hpc-sdk)

[3] [Nvidia Developer - Table of GPU Compute Capabilities](https://developer.nvidia.com/cuda-gpus#compute)

[4] [Nvidia CUDA 11.0.3 Toolkit Documentation - Volta Tuning Guide](https://docs.nvidia.com/cuda/archive/11.0/volta-tuning-guide/index.html)

[5] [Nvidia CUDA 11.2.0 Toolkit Documentation - Volta Tuning Guide](https://docs.nvidia.com/cuda/archive/11.2.0/volta-tuning-guide/index.html)