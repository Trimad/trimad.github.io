+++
title = "Simple 2D Terrain Generation Using Perlin Noise"
author = "Tristan Madden"
tags = ["interactive","JavaScript","p5.js","perlin noise","terrain"]
categories = ["JavaScript"]
date = "2017-07-16"
+++

<div class="iframe-wrapper-1-1">
    <iframe src="https://editor.p5js.org/Berkanan/full/LbNSvlqKU"></iframe>
</div>
<center><em>click mouse in iframe to generate a new map</em></center>
<br>
This sketch maps perlin noise between a value of 0 and 255 across a grid. Values greater than or equal to 100 are
"grass", values between 75 and 100 are "sand", and values less than or equal to 75 are "water".
<h2><a href="https://editor.p5js.org/Berkanan/full/LbNSvlqKU" target="_blank">Click here to render a fullscreen map</a>
</h2>
<h2><a href="https://editor.p5js.org/Berkanan/sketches/LbNSvlqKU">Launch the p5.js editor</a></h2>