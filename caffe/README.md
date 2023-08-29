---
title: Caffe
permalink: /Caffe/
---

**Caffe** is "a deep learning framework made with expression, speed, and
modularity in mind. It is developed by Berkeley AI Research (BAIR) and
by community contributors."[1]

Installed Version
-----------------

**Caffe 1.0** is installed on Proteus. It relies on Intel Math Kernel
Library (MKL) for linear algebra operations. It does not use CUDA or
OpenCV. To use, load the modulefile:

`   caffe/intel/1.0`

### Known Issues

-   Will only run on Intel nodes
-   Uses a private version of Python 2.7.13 - will conflict with other
    installed Python modules

Compiling Caffe
---------------

See: [Compiling Caffe](/Compiling_Caffe "wikilink")

GPU-enabled Caffe
-----------------

Besides the original Caffe, there are two forks available: NVCaffe, and
Caffe2.

-   The original version of Caffe is able to use only a **single** GPU
    device at a time.
-   NVIDIA's fork of Caffe, called NVCaffe,[2] is able to use multiple
    GPU devices simultaneously, using the NVIDIA Collective
    Communications Library (NCCL) for efficient multi-GPU and multi-node
    communication.[3]
-   Caffe2[4] (developed by Facebook), is another fork of Caffe, and it
    provides multi-GPU capability using NCCL. It has recently been
    merged with PyTorch.[5] PyTorch provides tensors and dynamic neural
    networks with strong GPU acceleration.

### Available Versions

Different versions/forks of Caffe are available via conda
environments.[6] For a summary of conda environments, see [Python\#Conda Environments](/Python#Conda_Environments "wikilink")

-   Caffe -- use the conda environment **`caffe`**
-   Caffe2 + PyTorch -- use the conda environment **`caffe2`**

The Open Neural Network Exchange (ONNX)[7] is an open format to
represent deep learning models, allowing different frameworks to
interoperate.

See Also
--------

-   [GPU Jobs](/GPU_Jobs "wikilink")

Example Job Scripst
-------------------

-   [Job Script Example 06 Caffe](/Job_Script_Example_06_Caffe "wikilink")

References
----------

<references/>

[1] [Caffe official website](http://caffe.berkeleyvision.org)

[2] [NVIDIA/caffe at GitHub](https://github.com/NVIDIA/caffe)

[3] [NVIDIA Collective Communications Library (NCCL)](https://developer.nvidia.com/nccl)

[4] [Caffe2 website](http://caffe2.ai)

[5] [PyTorch website](https://pytorch.org)

[6] [Conda User Guide: Managing Environments](https://conda.io/docs/user-guide/tasks/manage-environments.html)

[7] [Open Neural Network Exchange (ONNX) website](http://onnx.ai)