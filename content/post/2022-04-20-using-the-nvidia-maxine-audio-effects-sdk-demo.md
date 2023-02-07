---
title: Using the NVIDIA Maxine Audio Effects SDK Demo
author: Tristan Madden
categories: [NVIDIA, Maxine]
tags: [audio, sound, SDK]
---

This is my tentative workflow for cleaning-up poor audio using the NVIDIA Maxine SDK.
<h2><a href="https://developer.nvidia.com/maxine-getting-started">Download the Latest Audio Effects SDK
</a></h2>
<h2><a href="https://docs.nvidia.com/deeplearning/maxine/audio-effects-sdk/index.html">NVIDIA Maxine Documentation</a></h2>

1. Find and download a YouTube video using [youtube-dl]({{ site.baseurl }}{% link _posts/2022-04-16-youtube-dl.md %})
```console
youtube-dl -f bestaudio https://www.youtube.com/watch?v=XlfcvUtUoOM
```
2. The Audio Effects SDK only accepts audio that is in .wav format sampled at 8000Hz single-channel, 16000Hz single-channel, or 48000Hz single-channel. This can be resampled using [ffmpeg]({{ site.baseurl }}{% link _posts/2022-04-16-ffmpeg.md %}).
- Convert a .mp3 file to 8kHz, single-channel PCM:
```console
ffmpeg -i test.mp3 -ar 8000 -ac 1 input.wav
```
- Convert a .m4a file to 16kHz, single-channel PCM:
```console
ffmpeg -i "Elvis How great thou art 1972 (very impressive)-XlfcvUtUoOM.m4a" -ar 16000 -ac 1 input.wav
```
- Convert a .wav file to 48kHz, single-channel PCM:
```console
ffmpeg -i test.wav -ar 48000 -ac 1 -input.wav
```
3. Create a config file to use with the effects_demo included with the SDK. Below is an example config "myconfig.txt" that combines two pre-trained models provided by the SDK to reduce reverb, to reduce noise, and to create an upscaled, <em>supperresolution</em> version of the audio. 
```console
# Effect
effect dereverb_denoiser16k_superres16kto48k
# Point this to the model file.
model ..\..\bin\models\turing\dereverb_denoiser_16k.trtpkg,..\..\bin\models\turing\superres_16kto48k.trtpkg
# Input file
input_wav input.wav
# Effect applied audio data will be saved to this file.
output_wav output.wav
# Set to 1 for real time mode i.e. audio data will be processed 
# at same speed like that of an audio input device like
# microphone. Since the denoising is faster that real time, the
# processing will be equal to audio file duration.
real_time 0
# Intensity Ratio
intensity_ratio 1.0,1.0
```
4. Run the NVIDIA effects_demo with the aforementioned config file.
```console
effects_demo.exe -c myconfig.txt
```