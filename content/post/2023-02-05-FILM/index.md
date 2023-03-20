---
author: Tristan Madden
categories: [Interpolation]
date: 2023-02-05
featured: true
lastmod: 2023-02-27
summery: "\"The official Tensorflow 2 implementation of our high quality frame interpolation neural network. We present a unified single-network approach that doesn't use additional pre-trained networks, like optical flow or depth, and yet achieve state-of-the-art results. We use a multi-scale feature extractor that shares the same convolution weights across the scales. Our model is trainable from frame triplets alone.\""
tags: [ai, video]
title: FILM
thumbnail: "thumbnail.png"
usePageBundles: true
---

<h3><a href="https://github.com/google-research/frame-interpolation">GitHub Repository</a></h3>

<h3>Setup GPU environment</h3>
The Windows setup instructions in the GitHub repository are wrong or outdated. Tensorflow kept trying to use the CPU instead of the GPU until I installed CUDA and CUDNN this way:

```Shell
conda install -c conda-forge cudatoolkit=11.2 cudnn=8.1.0
python -m pip install "tensorflow<2.11"
pip install protobuf==3.20.3
```

<h3>Usage</h3>

Activate the Conda environment:

```Shell
conda activate frame_interpolation
```

From the Conda Shell, cd to the FILM directory:

```Shell
cd C:\Users\trima\frame-interpolation
```

Open File Explorer at this directory and copy the frames you want to interpolate to the "photos" folder.

```Shell
start .
```

Place the images you would like to interpolate in the "photos" directory and run this command to begin interpolating them:

```Shell
python -m eval.interpolator_cli --pattern "photos" --model_path pretrained_models\film_net\Style\saved_model --times_to_interpolate 1 --output_video
```