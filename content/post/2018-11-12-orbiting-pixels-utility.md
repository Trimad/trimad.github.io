---
title: Orbiting Pixels Utility
author: Tristan Madden
categories: [JavaScript,p5.js]
tags: [processing, interactive]
date: 2018-11-12
---
<div class="iframe-wrapper-1-1">
    <iframe src="https://editor.p5js.org/Berkanan/full/P61irisoa"></iframe>
</div>
<h2><a href="https://editor.p5js.org/Berkanan/full/P61irisoa" target="_blank">Launch this sketch is a new window (recommended)</a></h2>

<h2><a href="https://editor.p5js.org/Berkanan/sketches/P61irisoa">Launch p5.js web editor</a></h2>
This is not a program that should be running in a browser. With that said, writing this program was a laborious process that taught me a lot about JavaScript, about Processing, and about optimization. The algorithm I implemented works first by creating a grid of evenly-spaced points. Each points has a corresponding object that rotates around that point that can be assigned characteristics such as rotation radius, a shape, and a color that corresponds to an x and y position on a source image. To make things more interesting, I mapped the phase of each object's rotation to the brightness value of the source image's corresponding pixel. The results... could probably be better summarized with a YouTube demonstration.
<div class="iframe-wrapper-16-9">
    <iframe src="https://www.youtube.com/embed/wScbpI7JfZ0?feature=oembed"></iframe>
</div>
<br>
<div class="iframe-wrapper-16-9">
    <iframe src="https://www.youtube.com/embed/-dvcPju6q5o?feature=oembed"></iframe>
</div>