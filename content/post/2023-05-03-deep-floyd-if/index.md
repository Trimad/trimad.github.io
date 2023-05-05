---
author: Tristan Madden
categories: [Python, Shell]
date: 2023-05-03
draft: false
tags: [ai, images]
title: Deep Floyd IF
thumbnail: "thumbnail.png"
summary: My tentative work flow for running Deep Floyd IF locally with all safety features removed.
usePageBundles: true
toc: true
---

## Download prerequisites
1. [Miniconda](https://docs.conda.io/en/latest/miniconda.html)
2. [Git](https://git-scm.com/download/win)

## Setup Environment
### Clone the git repo

```Shell
git clone https://github.com/deep-floyd/IF.git
```

### cd to the repo folder
_In my case:_
```Shell
cd C:\Users\trima\Documents\GitHub\IF
```

### Create the conda environment
```Shell
conda create --name IF python=3.10.10
```

### Activate the environment
```Shell
conda activate IF
```

### Install requirements
```Shell
pip install -r requirements.txt --upgrade
pip3 install --pre torch torchvision torchaudio --index-url https://download.pytorch.org/whl/nightly/cu118
pip3 install --pre torch torchvision torchaudio --index-url https://download.pytorch.org/whl/nightly/cu117 #bitsandbytes doesn't support cu118 yet
```

## Setup Program

### Download the model weights from Hugging Face
To keep things tidy I put them in a folder called cache.
```Shell
mkdir cache
cd cache
```
```Shell
git clone https://huggingface.co/DeepFloyd/IF-I-XL-v1.0.git
```
```Shell
git clone https://huggingface.co/DeepFloyd/IF-II-L-v1.0.git
```
```Shell
git clone https://huggingface.co/stabilityai/stable-diffusion-x4-upscaler.git
```

* IF-I-XL-v1.0.git is ~262 GB
* IF-II-L-v1 is ~182 GB
* stable-diffusion-x4-upscaler is ~26.1 GB

## Run Deep Floyd IF
Put the code below in a file called run.py. Run it in Anaconda Prompt with `python run.py`
```Python

import gc
import torch
import time

torch.cuda.set_per_process_memory_fraction(0.5)

def flush():
    gc.collect()
    torch.cuda.empty_cache()

from diffusers import DiffusionPipeline
from diffusers.utils import pt_to_pil

# stage 1
stage_1 = DiffusionPipeline.from_pretrained("./IF-I-XL-v1.0", variant="fp16", torch_dtype=torch.float16, safety_checker=None)

# stage 2
stage_2 = DiffusionPipeline.from_pretrained('./IF-II-L-v1.0', text_encoder=None, variant="fp16", torch_dtype=torch.float16, safety_checker=None)

# stage 3
stage_3 = DiffusionPipeline.from_pretrained('./stable-diffusion-x4-upscaler', torch_dtype=torch.float16, safety_checker=None)

# Memory management
stage_1.enable_sequential_cpu_offload()
stage_2.enable_model_cpu_offload()
stage_3.enable_model_cpu_offload()

# prompt
prompt = 'an anime girl wearing a shirt that says "hello world"'

# text embeds
prompt_embeds, negative_embeds = stage_1.encode_prompt(prompt)

# seed settings
time_seed = int(time.time())
generator = torch.manual_seed(time_seed)

# stage 1
image = stage_1(prompt_embeds=prompt_embeds, negative_prompt_embeds=negative_embeds, generator=generator, output_type="pt").images
pt_to_pil(image)[0].save("./if_stage_I.png")

del stage_1
flush()

# stage 2
image = stage_2(
    image=image, prompt_embeds=prompt_embeds, negative_prompt_embeds=negative_embeds, generator=generator, output_type="pt"
).images
pt_to_pil(image)[0].save("./if_stage_II.png")

del stage_2
flush()

# stage 3
image = stage_3(prompt=prompt, image=image, generator=generator, noise_level=100).images
image[0].save("./if_stage_III.png")
```

