---
title: NGC Deep Learning Example
permalink: /NGC_Deep_Learning_Example/
---

NGC (Nvidia GPU Cloud) Deep Learning Example

-   <https://developer.nvidia.com/blog/how-to-run-ngc-deep-learning-containers-with-singularity/>
-   <https://ngc.nvidia.com/catalog/resources/nvidia:resnet_50_v1_5_for_tensorflow/quickStartGuide>

ResNet data is in:

`  /beegfs/ILSVRC`

On Picotte
----------

``` text
$ srun -p gpu --gres=gpu:4 --cpus-per-gpu=12 --mem=128G --time=8:00:00 --pty /bin/bash
gpu001$ singularity run --nv -B /beegfs/ILSVRC/Data/DET/train:/data/imagenet tensorflow-19.11-tf1-py3.sif python /workspace/nvidia-examples/resnet50v1.5/main.py --mode=training_benchmark --use_tf_amp --warmup_steps=200 --batch_size=256 --data_dir=/data/imagenet --results_dir=/beegfs/scratch/myname/resnet
```