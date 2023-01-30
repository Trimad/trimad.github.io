---
author: Tristan Madden
categories: [Stable Diffusion]
date: 2023-01-27
tags: [🎨]
title: Stable Diffusion Scripts
---

<h2>Data Grooming</h2>

<h3>Numbering PNG files in a folder in sequence</h3>

```Python
import os
import pathlib

collection = os.getcwd()
#print(collection)
for i, filename in enumerate(os.listdir(collection)):
    file_extension = pathlib.Path(filename).suffix
    #print("File Extension: ", file_extension)
    if(file_extension == ".png"):
        os.rename(collection +"\\"+ filename, collection +"\\"+ str(i).zfill(4) + ".png")
```
This code block is a Python script that does the following:

1. Imports the os and pathlib modules.
2. Defines a variable collection which is set to the current working directory.
3. Iterates through all files in the current working directory using os.listdir(collection) and enumerate() function
4. For each file, it gets the file extension using the pathlib.Path(filename).suffix method
5. If the file extension is ".png", it renames the file using os.rename() function. The new file name is a zero-padded four digit number, followed by ".png".
The script is changing the name of all png files in the current working directory to a 4 digit zero padded number followed by .png, this is useful if you have a sequence of files in a folder and you want to sort them in order and not have any gaps in the numbering.

<h3>Distance Sort </h3>

```Python
import os
from PIL import Image
from math import sqrt

folder_path = 'C:/Users/trima/FILM/photos'
image_files = os.listdir(folder_path)

# Open the first image and get its RGB values
first_image = Image.open(os.path.join(folder_path, image_files[0]))
first_image_rgb = first_image.getdata()

# Create a list to store the distances
distances = []

# Iterate over the remaining images in the folder
for image_file in image_files[1:]:
    if image_file.endswith('.png'):
        image = Image.open(os.path.join(folder_path, image_file))
        image_rgb = image.getdata()
        distance = 0
        for i in range(len(first_image_rgb)):
            distance += sqrt((first_image_rgb[i][0]-image_rgb[i][0])**2 + (first_image_rgb[i][1]-image_rgb[i][1])**2 + (first_image_rgb[i][2]-image_rgb[i][2])**2)
        print((distance, image_file))
        distances.append((distance, image_file))

# Sort the distances list by distance
distances.sort()

# Rename the first image to "0000.png"
os.rename(os.path.join(folder_path, image_files[0]), os.path.join(folder_path, "0000.png"))

# Rename each image to the next number in the sequence
for i in range(len(distances)):
    os.rename(os.path.join(folder_path, distances[i][1]), os.path.join(folder_path, str(i+1).zfill(4) + '.png'))
```

This script is a Python script that renames a sequence of PNG images in a folder based on the distance of their RGB values from the RGB values of the first image in the sequence. The script imports the os, PIL and math modules, it sets the folder path where the images are located and get the list of all files in that folder. It opens the first image and gets its RGB values, then creates an empty list to store the distances. It iterates over the remaining images in the folder, if the file is a png it opens the image and gets its RGB values. Then it calculates the distance between the RGB values of the current image and the first image using the Euclidean distance formula, appends the distance and the image name to the distance list. The script then sorts the distance list by the distance, renames the first image to "0000.png" and renames each image to the next number in the sequence using the os.rename() function, and 4 digits zero-padded number followed by the extension.

<h2>Frame Interpolation</h2>

<h3><a href="https://github.com/google-research/frame-interpolation" title="FILM">FILM</a></h3>


Activate the Conda environment
```Shell
conda activate frame_interpolation
```
From the Conda Shell, cd to the FILM directory
```Shell
cd C:\Users\trima\FILM
```
Place the images you would like to interpolate in the "photos" directory and run this command to begin interpolating them.
```Shell
python -m eval.interpolator_cli --pattern "photos" --model_path pretrained_models\film_net\Style\saved_model --times_to_interpolate 1 --output_video
```

<h2>Color Grading</h3>

```Python
import os
import cv2
import numpy as np

def average_color_grading(folder_path):
    # Get all image filenames in the folder
    filenames = [f for f in os.listdir(folder_path) if f.endswith('.jpg') or f.endswith('.png')]
    
    # Initialize a sum of color grading for all images
    color_grading_sum = None
    
    # Iterate through all images, adding each image's color grading to the sum
    for filename in filenames:
        print("averaging " + filename)
        image_path = os.path.join(folder_path, filename)
        image = cv2.imread(image_path)
        
        # Average color grading of an image is computed as mean of its pixels
        color_grading = np.mean(image, axis=(0, 1))
        
        # Add the color grading of the current image to the sum
        if color_grading_sum is None:
            color_grading_sum = color_grading
        else:
            color_grading_sum += color_grading
    
    # Divide the sum of color grading by the number of images to get the average color grading
    average_color_grading = color_grading_sum / len(filenames)
    
    return average_color_grading

def apply_color_grading(folder_path, average_color_grading):
    # Get all image filenames in the folder
    filenames = [f for f in os.listdir(folder_path) if f.endswith('.jpg') or f.endswith('.png')]
    
    # Create a new folder to save the color graded frames
    color_graded_folder = os.path.join(folder_path, 'color_graded')
    os.makedirs(color_graded_folder, exist_ok=True)
    
    # Iterate through all images, applying the average color grading to each frame
    for i, filename in enumerate(filenames):
        print("color grading " + filename)
        image_path = os.path.join(folder_path, filename)
        image = cv2.imread(image_path)
        
        # Subtract the average color grading from each pixel to apply the color grading
        color_graded_image = image - np.mean(image, axis=(0, 1)) + average_color_grading
        
        # Zero-pad the sequential number and save the color graded image with the zero-padded sequential number
        color_graded_image_path = os.path.join(color_graded_folder, str(i).zfill(len(str(len(filenames)))) + '.jpg')
        cv2.imwrite(color_graded_image_path, color_graded_image)

folder_path = 'images'
average_color_grading = average_color_grading(folder_path)
apply_color_grading(folder_path, average_color_grading)
```
This program applies color grading to a set of images stored in the "images" folder. It does so by first computing the average color grading of all the images and then subtracting the average color grading from each pixel of each image and adding the average color grading. The resulting color graded images are saved in a new folder called "color_graded" within the "images" folder. It applies the average color grading to each frame by subtracting the mean of each frame's pixels from each pixel and adding the average color grading. 