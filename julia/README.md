---
title: Julia
permalink: /Julia/
---

**Julia** is a programming language designed for high performance.[1]

Installed Versions
------------------

Multiple versions of **Julia** are installed on Picotte. Use the
appropriate
[modulefile](/Environment_Modules_Quick_Start_Guide "wikilink"):

`    julia/1.5.2`
`    julia/1.5.3`
`    julia/1.6.0`
`    julia/1.6.5`
`    julia/1.6.7 (LTS version)`
`    julia/1.7.1`
`    julia/1.7.3`
`    julia/1.8.2`
`    julia/1.8.3`
`    julia/1.8.4`

“`module avail julia`” will show all available versions.

Jupyter Notebook
----------------

-   See: [Julia in Jupyter](/Julia_in_Jupyter "wikilink")

Accelerated math/linear algebra with MKL
----------------------------------------

[MKL is Intel's Math Kernel Library](https://www.intel.com/content/www/us/en/developer/tools/oneapi/onemkl.html)

See: <https://github.com/JuliaLinearAlgebra/MKL.jl>

``` text
julia> using Pkg; Pkg.add("MKL")
```

GPUs using CUDA
---------------

See: <https://cuda.juliagpu.org/stable/>

Example:

``` text
[juser@picotte001 ~]$ srun -p gpu --gpus=1 --time=1:00:00 --mem=12G --ntasks=12 --pty /bin/bash
[juser@gpu002 ~]$ module load julia
[juser@gpu002 ~]$ julia
               _
   _       _ _(_)_     |  Documentation: https://docs.julialang.org
  (_)     | (_) (_)    |
   _ _   _| |_  __ _   |  Type "?" for help, "]?" for Pkg help.
  | | | | | | |/ _` |  |
  | | |_| | | | (_| |  |  Version 1.8.3 (2022-11-14)
 _/ |__'_|_|_|__'_|  |  Official https://julialang.org/ release
|__/                   |

julia> using CUDA

julia> CUDA.versioninfo()
  Downloaded artifact: CUDA_compat
  Downloaded artifact: CUDA
CUDA toolkit 11.7, artifact installation
NVIDIA driver 470.57.2, for CUDA 11.4
CUDA driver 11.7

Libraries:
- CUBLAS: 11.10.1
- CURAND: 10.2.10
- CUFFT: 10.7.2
- CUSOLVER: 11.3.5
- CUSPARSE: 11.7.3
- CUPTI: 17.0.0
- NVML: 11.0.0+470.57.2
  Downloaded artifact: CUDNN
- CUDNN: 8.30.2 (for CUDA 11.5.0)
  Downloaded artifact: CUTENSOR
- CUTENSOR: 1.4.0 (for CUDA 11.5.0)

Toolchain:
- Julia: 1.8.3
- LLVM: 13.0.1
- PTX ISA support: 3.2, 4.0, 4.1, 4.2, 4.3, 5.0, 6.0, 6.1, 6.3, 6.4, 6.5, 7.0, 7.1, 7.2
- Device capability support: sm_35, sm_37, sm_50, sm_52, sm_53, sm_60, sm_61, sm_62, sm_70, sm_72, sm_75, sm_80, sm_86

1 device:
  0: Tesla V100-SXM2-32GB (sm_70, 31.745 GiB / 31.749 GiB available)
julia> Pkg.test("CUDA")
...
┌ Info: System information:
│ CUDA toolkit 11.7, artifact installation
│ NVIDIA driver 470.57.2, for CUDA 11.4
│ CUDA driver 11.7
│
│ Libraries:
│ - CUBLAS: 11.10.1
│ - CURAND: 10.2.10
│ - CUFFT: 10.7.2
│ - CUSOLVER: 11.3.5
│ - CUSPARSE: 11.7.3
│ - CUPTI: 17.0.0
│ - NVML: 11.0.0+470.57.2
│ - CUDNN: 8.30.2 (for CUDA 11.5.0)
│ - CUTENSOR: 1.4.0 (for CUDA 11.5.0)
│
│ Toolchain:
│ - Julia: 1.8.3
│ - LLVM: 13.0.1
│ - PTX ISA support: 3.2, 4.0, 4.1, 4.2, 4.3, 5.0, 6.0, 6.1, 6.3, 6.4, 6.5, 7.0, 7.1, 7.2
│ - Device capability support: sm_35, sm_37, sm_50, sm_52, sm_53, sm_60, sm_61, sm_62, sm_70, sm_72, sm_75, sm_80, sm_86
│
│ 1 device:
└   0: Tesla V100-SXM2-32GB (sm_70, 31.745 GiB / 31.749 GiB available)
[ Info: Testing using 1 device(s): 0. Tesla V100-SXM2-32GB (UUID 554db877-f017-2c7f-5ed4-af8fc0e606ad)
  Downloaded artifact: cuQuantum
                                                  |          | ---------------- GPU ---------------- | ---------------- CPU ---------------- |
Test                                     (Worker) | Time (s) | GC (s) | GC % | Alloc (MB) | RSS (MB) | GC (s) | GC % | Alloc (MB) | RSS (MB) |
initialization                                (2) |     6.94 |   0.00 |  0.0 |       0.00 |   335.00 |   0.02 |  0.3 |     108.20 |   824.66 |
gpuarrays/indexing scalar                     (2) |    35.96 |   0.00 |  0.0 |       0.01 |   373.00 |   1.38 |  3.9 |    4348.39 |   824.66 |
...
```

Julia with CUDA is able to utilize multiple GPU devices:

``` text
[juser@gpu005 ~]$ nvidia-smi
...
+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|    0   N/A  N/A      8044      C   ...opt/julia/1.8.3/bin/julia      557MiB |
|    1   N/A  N/A      8044      C   ...opt/julia/1.8.3/bin/julia      557MiB |
|    2   N/A  N/A      8044      C   ...opt/julia/1.8.3/bin/julia      557MiB |
|    3   N/A  N/A      8044      C   ...opt/julia/1.8.3/bin/julia     1063MiB |
+-----------------------------------------------------------------------------+
```

See Also
--------

-   Automatic Full Compilation of Julia Programs and ML Models to Cloud
    TPUs, K. Fischer & E. Saba,
    [arXiv:1810.09868](https://arxiv.org/abs/1810.09868) \[cs.PL\]
-   [GPU Jobs on Picotte](/GPU_Jobs_on_Picotte "wikilink")

References
----------

<references/>

[1] [Julia Language Official Website](https://julialang.org)