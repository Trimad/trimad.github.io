---
author: Tristan Madden
categories: [Python]
date: 2023-03-23
lastmod: 2023-09-07
draft: false
featured: true
summary: "TheraFit is a personal project and Large Language Model approach to matching clients to therapists."
tags: [Python]
thumbnail: "thumbnail.png"
title: "TheraFit"
toc: true
usePageBundles: true
---
TheraFit is a personal project and Large Language Model approach to matching clients to therapists.
## Environment Setup
Clone the <a href="https://github.com/Trimad/TheraFit">Git repository</a>.
```shell
git clone https://github.com/Trimad/TheraFit
```
Install <a href="https://docs.conda.io/projects/miniconda/en/latest">Miniconda</a>.


### Create the environment
```shell
conda create --name therafit --file environment.txt
```
```shell
conda activate therafit
```
### Install pip requirements
```shell
pip3 install -r requirements.txt
```

### Install the Windows version of bitsandbytes
```shell
python -m pip install bitsandbytes --prefer-binary --extra-index-url=https://jllllll.github.io/bitsandbytes-windows-webui 
```

### Force reinstall torch until it works
```shell
pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118 --force-reinstall
```

## Test the Environment
Use the test script included in the Git repo. 
```shell
python test.py
```
If the environment is configured correctly, Torch, Transformers, Gradio and Accelerate should all return True.
```shell
C:\Users\Tristan\Documents\GitHub\TheraFit>python test.py
Torch (CUDA availability): True
Transformers: True
Gradio: True
Accelerate: True
```

## Run Therafit
```shell
python run.py
```