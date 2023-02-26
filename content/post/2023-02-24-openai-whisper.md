---
author: Tristan Madden
categories: [CMD]
date: 2023-02-24
tags: [openai, whisper]
title: OpenAI Whisper
---

Whisper is a general-purpose speech recognition model. It is trained on a large dataset of diverse audio and is also a multi-task model that can perform multilingual speech recognition as well as speech translation and language identification.

<h3><a href="<h2><a href="https://github.com/google-research/frame-interpolation" title="FILM">GitHub Repository</a></h3>

<h3>Installation</h3>

```CMD
pip install git+https://github.com/openai/whisper.git 
```

<h3>Fix CUDA not detecting GPU</h3>

```CMD
pip uninstall torch
pip install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu116
```

<h3>Usage</h3>

```CMD
whisper input.mp3 --model medium --language en --task transcribe
```