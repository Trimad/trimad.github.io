---
author: Tristan Madden
categories: [Stable Diffusion]
date: 2023-01-27
tags: [🎨]
title: Stable Diffusion Scripts
---

<h2>Data Grooming</h2>

<h3>Numbering PNG files in a folder in sequence</h3>

```JavaScript
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

```JavaScript
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