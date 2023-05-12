---
author: Tristan Madden
categories: [python]
date: 2023-02-05
featured: true
featureImage: interpolated.gif
lastmod: 2023-05-11
summary: "*\"The official Tensorflow 2 implementation of our high quality frame interpolation neural network. We present a unified single-network approach that doesn't use additional pre-trained networks, like optical flow or depth, and yet achieve state-of-the-art results. We use a multi-scale feature extractor that shares the same convolution weights across the scales. Our model is trainable from frame triplets alone.\"*"
tags: [ai, video]
toc: true
title: Frame Interpolation Large Motion (FILM)
thumbnail: interpolated.gif
usePageBundles: true
---

*\"The official Tensorflow 2 implementation of our high quality frame interpolation neural network. We present a unified single-network approach that doesn't use additional pre-trained networks, like optical flow or depth, and yet achieve state-of-the-art results. We use a multi-scale feature extractor that shares the same convolution weights across the scales. Our model is trainable from frame triplets alone.\"*

## Prerequisites

### Miniconda

<a href="https://docs.conda.io/en/latest/miniconda.html" title="https://docs.conda.io/en/latest/miniconda.html">https://docs.conda.io/en/latest/miniconda.html</a>

### Git

<a href="https://git-scm.com/download/win" title="https://git-scm.com/download/win">https://git-scm.com/download/win</a>

## Setup

Get Frame Interpolation source codes:
```Shell
git clone https://github.com/google-research/frame-interpolation.git
```
cd into the cloned git repository, for example:
```Shell
cd C:\Users\trima\Documents\GitHub\frame-interpolation
```

Create the Miniconda virtual environment:
```Shell
conda create -n frame-interpolation pip python=3.9
```
Activate the Miniconda virtual environment:
```Shell
conda activate frame-interpolation
```
Install requirements:
```Shell
pip install -r requirements.txt
```
```Shell
conda install -c anaconda cudnn
```



## Usage



From the Conda Shell, cd to the FILM directory:



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

