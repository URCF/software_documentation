---
title: LIBSVM
permalink: /LIBSVM/
---

**LIBSVM** is a library for support vector machines.[1]

Installed Versions
------------------

Only an old CUDA-enabled LIBSVM is available on Proteus. This is
available only on GPU nodes. Use the modulefile:

`   libsvm/1.2.3`

This code was created by MKLab-ITI,[2] and updated by D. Chin. The fork
is available on GitHub.[3]

Note that the GPU-enabled LIBSVM only provides the training program. The
other programs (svm-predict, svm-scale) are provided by the standard
(CPU-only) LIBSVM. The standard LIBSVM version installed with this
GPU-enabled version is 317.

Usage
-----

There is only one GPU-enabled program installed with this CUDA-enabled
LIBSVM:

`   svm-train-gpu`

The other LIBSVM programs are CPU-only:

`   svm-scale`
`   svm-predict`

References
----------

<references/>

[1] [LIBSVM web page](https://www.csie.ntu.edu.tw/~cjlin/libsvm/)

[2] [MKLab-ITI CUDA-enabled LIBSVM GitHub Repository](https://github.com/MKLab-ITI/CUDA)

[3] <https://github.com/prehensilecode/mklabiti-libsvm-cuda>