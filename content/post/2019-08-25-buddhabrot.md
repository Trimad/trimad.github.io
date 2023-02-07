---
title: Buddhabrot
author: Tristan Madden
categories: [C, Fractal, Java]
tags: [fractal, buddhabrot]
date: 2019-08-25
---
<!-- ![Desktop View](https://i.imgur.com/FLfOIxr.jpg)

This image was produced by my C# implementation of the Buddhabrot algorithm. <a href="https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.parallel.for?view=net-6.0">Parallel.For</a> makes multithreaded CPU rendering so easy and readable that I see no reason to continue writing anything fractal related in Java. 

Some stats on this image:
- 8191x8191 pixels
- 32bpp, ARGB
- Histogram coloring algorithm
- 50 passes, 1500 bailout value
- 1:1 aspect ratio

<a href="https://github.com/Trimad/Sandbox">GitHub Repository</a> -->


<!-- 
![Desktop View](https://i.imgur.com/eLjfYTR.jpg)
This image was produced by my Java implementation of the Buddhabrot algorithm.  I was very interested in this fractal as a teenager and believe I first encountered it on <a href="http://www.complexification.net/gallery/machines/buddhabrot/">www.complexification.net</a> when I was 15 or so. 

 I needed a working implementation of rendering the Mandelbrot set before I could produce this image, so there are functions leftover in this program for generating the Mandelbrot set. I've implemented some basic features such as random Gaussian points to introduce some blur, HSB color support, and basic <a href="https://en.wikipedia.org/wiki/Misiurewicz_point">Misiurewicz Point</a> support. It's Java though, so the performance is abysmal.

Some stats on this image:
- HSB colorizing algorithm
- 7680x4320 pixels
- 3000 iterations
- 16:9 aspect ratio

<h1><a href="https://gitlab.com/tristan.madden/pixelplayground/tree/master/src/pixelsplayground">GitLab Repository</a></h1> -->