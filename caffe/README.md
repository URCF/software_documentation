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

Versions and Forks
------------------

-   The original Caffe is from Berkeley AI Research[1]. This uses GPUs
    (CUDA) with the limitation that only a single GPU device may be used
    at a time.
-   NVIDIA forked Caffe, adding some features, notably the ability to
    use more than one GPU device[2]
-   Caffe2 forked by Facebook[3][4]

Order of Operations
-------------------

### Prerequisite

-   cmake &gt;= 3.8
-   C++ 11 i.e. gcc &gt; 4.8

### Compilation Order

Some of these may be provided by system.

-   hdf5 1.8.x
-   gflags
-   glog
-   snappy
-   leveldb
-   lmdb
-   Python 2.7.13
    -   numpy
    -   scipy
    -   etc
-   scons - works only with Python 2.x
-   Boost
-   caffe
    -   NVML - /cm/shared/apps/cuda60/tdk/331.62/nvml

Configure
---------

There are two ways to do it:

-   the "official" way is to manually edit the Makefile.config at the
    top of the source tree.
-   the other way is to use the unofficial Cmake.

I find Cmake a little easier to deal with.

### Using ccmake

To use ccmake, a separate build directory must be created, and the build
run from there:

``` text
[juser@proteusi01 caffe-1.0]$ mkdir build
[juser@proteusi01 caffe-1.0]$ cd build
[juser@proteusi01 caffe-1.0]$ ccmake ..
```

Then, hit "c" to start the configure process; it will take a minute or
two. Then, once a list of options/cmake variables appear, hit "t" to
toggle on the advanced options. Use the cursor keys to navigate to any
variable you want to modify; hit "Enter" to start editing, and hit
"Return" once done. Repeat the editing process for all variables you
need to modify.

With some variables, once a modification is made, hitting "c" to
configure will pull in more variables, e.g. for using Intel MKL the
variables setting various MKL library locations will come up. Keep
hitting "c" until the option "g" to generate appears. It should take
only one or two iterations. Once you hit "g", the makefile is generated
and the cmake configurator will exit.

Then, follow the rest of the caffe build instructions[5]

``` text
make
make pycaffe
make runtest
make install
```

NVCaffe
-------

-   NVML location - /cm/shared/apps/cuda60/tdk/331.62/nvml

References
----------

<references/>

[1] [Berkeley AI Research - Caffe official website](http://caffe.berkeleyvision.org/)

[2] [NVIDIA caffe GitHub repository](https://github.com/NVIDIA/caffe)

[3] [Caffe2 official website](https://caffe2.ai/)

[4] [NVIDIA Dev Blog - Caffe2: Portable High-Performance Deep Learning Framework from Facebook](https://devblogs.nvidia.com/parallelforall/caffe2-deep-learning-framework-facebook/)

[5] [Caffe documentation - RHEL/Fedora/CentOS Installation](http://caffe.berkeleyvision.org/install_yum.html)

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
