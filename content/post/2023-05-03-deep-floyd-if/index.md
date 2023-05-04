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
pip install -r requirements.txt
```

## Setup Program

### Download the model weights from Hugging Face
To keep things tidy I put them in a folder called cache.
```Shell
cd C:\Users\trima\Documents\GitHub\IF\cache
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
### Run Deep Floyd IF

```Python
from diffusers import DiffusionPipeline
from diffusers.utils import pt_to_pil
import torch

# stage 1
stage_1 = DiffusionPipeline.from_pretrained('./cache/models--DeepFloyd--IF-II-L-v1.0', variant="fp16", torch_dtype=torch.float16)
stage_1.enable_model_cpu_offload()

# stage 2
stage_2 = DiffusionPipeline.from_pretrained(
    './cache/models--DeepFloyd--IF-I-XL-v1.0', text_encoder=None, variant="fp16", torch_dtype=torch.float16
)
stage_2.enable_model_cpu_offload()

# stage 3
stage_3 = DiffusionPipeline.from_pretrained('./cache/models--DeepFloyd--IF-I-XL-v1.0', torch_dtype=torch.float16)
stage_3.enable_model_cpu_offload()

prompt = 'a photo of a kangaroo wearing an orange hoodie and blue sunglasses standing in front of the eiffel tower holding a sign that says "very deep learning"'

# text embeds
prompt_embeds, negative_embeds = stage_1.encode_prompt(prompt)

generator = torch.manual_seed(0)

# stage 1
image = stage_1(prompt_embeds=prompt_embeds, negative_prompt_embeds=negative_embeds, generator=generator, output_type="pt").images
pt_to_pil(image)[0].save("./if_stage_I.png")

# stage 2
image = stage_2(
    image=image, prompt_embeds=prompt_embeds, negative_prompt_embeds=negative_embeds, generator=generator, output_type="pt"
).images
pt_to_pil(image)[0].save("./if_stage_II.png")

# stage 3
image = stage_3(prompt=prompt, image=image, generator=generator, noise_level=100).images
image[0].save("./if_stage_III.png")

```