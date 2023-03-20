---
title: Generating Terrain with Open Simplex Noise
author: Tristan Madden
categories: [Java, Processing]
tags: [open simplex noise]
date: 2018-01-03
usePageBundles: true
thumbnail: "thumbnail.png"
draft: true
---

<!-- 

![Desktop View](https://res.cloudinary.com/deiub7j41/image/upload/v1648703328/image_28-08-2019-06-03-27_oknl1s.jpg)
Expanding on last night's work with <a href="https://en.wikipedia.org/wiki/OpenSimplex_noise">Open Simplex Noise</a>. I
figured the next logical step was to make a Minecraftian terrain generator, so here it is. If I ever felt compelled to
build a game from the ground up, this would probably be my starting point.
<div class="iframe-wrapper-16-9">
    <iframe src="https://www.youtube.com/embed/-PTirgC0WX8">
    </iframe>
</div>
Just to further explore the idea of making a game, I threw some random walkers on my random terrain to see what
performance looked like. I was getting only 30FPS with 64 walkers on a relatively beefy CPU, confirming the obvious fact
that Java is the wrong language for this application.
<div class="iframe-wrapper-16-9">
    <iframe src="https://www.youtube.com/embed/qxbR32r74no">
    </iframe>
</div>
<h2>FFMPEG settings used to assemble the rendered frames</h2>
```console
ffmpeg -r 60 -f image2 -s 3840x2160 -i frame-%04d.tif -c:v libx264 -preset slow -profile:v high -crf 9 -coder 1 -pix_fmt
yuv420p -movflags +faststart -g 30 -bf 2 -c:a aac -b:a 384k -profile:a aac_low test4.mp4
```
<h2>FFMPEG settings used to add music to the video</h2>
```console
ffmpeg -i test.mp4 -i music.mp3 -codec copy -shortest output.mp4
``` 

-->