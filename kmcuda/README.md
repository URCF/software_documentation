---
title: Kmcuda
permalink: /Kmcuda/
---

**kmcuda** is a large scale K-means and K-nn implementation on NVIDIA
GPU / CUDA.[1] The implementation is based on Ding, et al.[2]

Installed Version
-----------------

The latest main branch (as of 2019-11-22) is installed on the GPU nodes.
It is installed in the `python37` conda environment, using the
`python/anaconda3` modulefile.

`   $ module load python/anaconda3`
`   $ conda activate python37`
`   (python37) $ `

See [Anaconda\#Conda Environments](/Anaconda#Conda_Environments "wikilink") for infomation on
using conda environments.

References
----------

<references/>

[1] [kmcuda GitHub repository](https://github.com/src-d/kmcuda)

[2] [Yinyang K-Means: A Drop-In Replacement of the Classic K-Means with Consistent Speedup](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/ding15.pdf) (PDF)