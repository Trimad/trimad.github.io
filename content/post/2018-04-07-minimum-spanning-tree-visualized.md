---
title: Minimum Spanning Tree Visualized
author: Tristan Madden
categories: [Java, Processing]
tags: [prim's algorithm, minimum spanning tree]
date: 2018-04-07
---
<!-- ![Desktop View](https://res.cloudinary.com/deiub7j41/image/upload/v1648776746/image_28-08-2019-06-25-51_mh1llq.jpg)
I'm working on a few programs that I will be using using to visualize algorithms on procedurally generated terrain. This particular program visualizes a minimum spanning tree using <a href="https://en.wikipedia.org/wiki/Prim%27s_algorithm">Prim's Algorithm</a> on random walkers. The terrain is 128x128 tiles, each containing 4 vertices on the negative y-axis that I applied a <a href="https://en.wikipedia.org/wiki/Perlin_noise">Perlin Noise</a> height map to. The map projection is orthographic, so all objects with the same dimension appear the same size, regardless of whether they are near or far from the camera. The frames were rendered at 1920x1080 resolution.

<div class="iframe-wrapper-16-9">
    <iframe src="https://www.youtube.com/embed/RwauA6p1Cic"></iframe>
</div>



<h2>FFMPEG settings used to assemble the rendered frames</h2>
```console
ffmpeg -f image2 -r 60 -i frame-%04d.tif -vcodec libx264 -profile:v high444 -refs 16 -crf 0 -preset ultrafast test.mp4
```
<h2>FFMPEG settings used to add music to the video</h2>
```console
ffmpeg -i test.mp4 -i music.mp3 -codec copy -shortest output.mp4
``` -->