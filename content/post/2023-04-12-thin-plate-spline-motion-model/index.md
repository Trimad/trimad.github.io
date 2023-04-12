---
author: Tristan Madden
categories: [AI]
date: 2023-04-12
draft: false
summary: "This is my tentative workflow for using this repository."
tags: [LLaMA]
thumbnail: "thumbnail.gif"
title: "Thin Plate Spline Motion Model"
toc: true
usePageBundles: true
---

## <a href="https://github.com/yoyo-nb/Thin-Plate-Spline-Motion-Model">GitHub Repository</a>

### Conda Environment and Usage

```Shell
conda activate thin-plate-spline
```
```Shell
cd C:\Users\trima\Documents\GitHub\Thin-Plate-Spline-Motion-Model
```
```Shell
python demo.py --config config/vox-256.yaml --checkpoint checkpoints/vox.pth.tar --source_image assets/source.png --driving_video assets/driving.mp4
```
```Shell
python demo.py --config config/vox-256.yaml --checkpoint checkpoints/vox.pth.tar --find_best_frame --source_image assets/0014.png --driving_video assets/driving.mp4 --result_video output.mp4
```

### Initial Tests

{{< youtube p4PoMEeVxoU >}}

As far as I can tell, this program requires a `driving_video` that is 1:1 aspect ratio.
![This was the source_image used for the video above.](original.png)




