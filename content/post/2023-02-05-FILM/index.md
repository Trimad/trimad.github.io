---
author: Tristan Madden
categories: [Interpolation]
date: 2023-02-05
featured: true
lastmod: 2023-03-30
summery: "\"The official Tensorflow 2 implementation of our high quality frame interpolation neural network. We present a unified single-network approach that doesn't use additional pre-trained networks, like optical flow or depth, and yet achieve state-of-the-art results. We use a multi-scale feature extractor that shares the same convolution weights across the scales. Our model is trainable from frame triplets alone.\""
tags: [ai, video]
toc: true
title: FILM
thumbnail: "thumbnail.png"
usePageBundles: true
---

<h3><a href="https://github.com/google-research/frame-interpolation">GitHub Repository</a></h3>

## GPU Bug Fix
The Windows setup instructions in the GitHub repository are wrong or outdated. Tensorflow kept trying to use the CPU instead of the GPU until I installed CUDA and CUDNN this way:

```Shell
conda install -c conda-forge cudatoolkit=11.2 cudnn=8.1.0
python -m pip install "tensorflow<2.11"
pip install protobuf==3.20.3
```

## Usage

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

## Batch Processing

Enter this For loop in the Anaconda Shell to iterate through a folder of folders containing video frames and batch interpolate all of them.

```Shell
FOR /D %i IN ("C:\Users\<user>\<some>\<directory>\*") DO python -m eval.interpolator_cli --pattern "%i" --model_path pretrained_models\film_net\Style\saved_model --times_to_interpolate 1 --output_video
```

Use this batch script to copy all "interpolated.mp4" files to the same directory as the script and rename them in sequential order.

```Shell
@echo off
setlocal enabledelayedexpansion
set /a "count=0"

for /r "." %%a in ("*interpolated.mp4") do (
    set /a "count+=1"
    set "filename=00!count!.mp4"
    copy "%%a" "!filename:~-6!"
)

echo Finished copying !count! files.
```

