## Welcome to My Blog

Hi all! I am Harsh Munshi, currently working at Graymatics inc as a Research and Development Engineer. My majority of task is based on Machine Learning and Image processing which as a whole is a real red hot topic in research right now! Let's collaborate, collate and contribute.


```
#!/bin/python

print("We are going to hack everything")
```

## Making a Neural Network

Right from the inception of LeNet (the first time when a convolutional neural network ref: http://cs231n.github.io/convolutional-networks/ ), the potential of this field was known but couldn't be fully explored since there was a lack of proper hardware. But with the advent of modern day computing power everything seems easier and faster than before. Competitions like Kaggle, Imagenet, DRAPA have further pushed researchers to step up and make something out of the box.

If you had to make a neural network in 2000, you would have had to design the architecture all by yourself. However, we are in 2017. There are loads of libraries like tensorflow, theano, keras, torch, caffe which makes your life really easy.

In this example we will make a simple classifier which can clasify multiple classes, in this case we choose two, say running and punching. Download couple of thousand images, 1000 for running and 1000 for punching. Now the most important part is to split the data according to the number of classes. Let's go stepwise:

## Step 1: Setup

Make a directory and create a data directory under that:

```
$ mkdir new_model && cd new_model
$ mkdir data && cd data
$ mkdir training && cd training
$ mkdir running && mkdir punching
$ cd ..
$ mkdir validation && cd validation
$ mkdir running && mkdir punching
```

## Step 2: Data Sorting

Collect the appropriate images and put 800 images each under training folder and put 200 images under validation folder. This means each sub-folder under training would have 800 images and each subfolder under validation would have 200 images making a total of 800 * 2 + 200 * 2 = 2000. The Heirarchy should look something like this:

|Data Folder|Data subfolder(s)|Number of Images|
|----|----|----|
|training|running|800|
||punching|800|
|validation|running|200|
||punching|200|

## Step 3: Image Resizing 

Every neural network assumes an image of a fixed size. In this case we will make the images of the dimension 150 * 150. To do this we will write a pseudo script using opencv which can help to resize and renumber all the images in a folder.

Go to the specific directory (for example running under training) and execute the following code. You can copy it there are run it.

```
import glob
import cv2, os
import numpy as np

file_number = 1
delete_command = "rm -rf "
for filename in glob.glob("*.jpg"):

        i = cv2.imread(filename)
        j = cv2.resize(i, (224,224))
        cv2.imwrite(str(file_number) + '.jpg', j)
        cmmd = delete_command + str(filename)
        os.system(cmmd)
        file_number += 1

```

Run the following command after the code to rename the files:

```
$ a=1;for i in `ls *.jpg|sort -V`; do new=$(printf "%04d.jpg" "$a");mv $i $new;let a=a+1;done
```
