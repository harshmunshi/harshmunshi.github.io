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
        j = cv2.resize(i, (150,150))
        cv2.imwrite(str(file_number) + '.jpg', j)
        cmmd = delete_command + str(filename)
        os.system(cmmd)
        file_number += 1
```
Run the following command after the code to rename the files:
```
$ a=1;for i in `ls *.jpg|sort -V`; do new=$(printf "%04d.jpg" "$a");mv $i $new;let a=a+1;done
```
Repeat this step for all folders, or write a code which can go to all the 4 directories and do the same (That's a challange for the readers).

## Step 4: Making the Neural Network

So after the hard toil with collecting the data and arranging it, now comes the main part: Designing the net itself. We will use keras with tensorflow backend to achieve this. It's a very very simple net and don't expect 99% accuracy out of it.

Importing the required files:
```
import cv2
from keras.preprocessing.image import ImageDataGenerator
from keras.models import Sequential
from keras.layers import Conv2D, MaxPooling2D
from keras.layers import Activation, Dropout, Flatten, Dense
from keras import backend as K
```
Defining the essential parameters:

```
# dimensions of our images.
img_width, img_height = 150, 150

train_data_dir = 'data/train'
validation_data_dir = 'data/validation'
nb_train_samples = 1600
nb_validation_samples = 400
epochs = 50
batch_size = 16
```

Defining the model:
```
if K.image_data_format() == 'channels_first':
    input_shape = (3, img_width, img_height)
else:
    input_shape = (img_width, img_height, 3)

model = Sequential()
model.add(Conv2D(32, (3, 3), input_shape=input_shape))
model.add(Activation('relu'))
model.add(MaxPooling2D(pool_size=(2, 2)))

model.add(Conv2D(32, (3, 3)))
model.add(Activation('relu'))
model.add(MaxPooling2D(pool_size=(2, 2)))

model.add(Conv2D(64, (3, 3)))
model.add(Activation('relu'))
model.add(MaxPooling2D(pool_size=(2, 2)))

model.add(Flatten())
model.add(Dense(64))
model.add(Activation('relu'))
model.add(Dropout(0.5))
model.add(Dense(1))
model.add(Activation('sigmoid'))
```
Compiling the model with a proper loss function:
```
model.compile(loss='binary_crossentropy',
              optimizer='rmsprop',
              metrics=['accuracy'])
```
Augmentation configuration for training, taing generator and validation generator:
```
train_datagen = ImageDataGenerator(
    rescale=1. / 255,
    shear_range=0.2,
    zoom_range=0.2,
    horizontal_flip=True)

# this is the augmentation configuration we will use for testing:
# only rescaling
test_datagen = ImageDataGenerator(rescale=1. / 255)

train_generator = train_datagen.flow_from_directory(
    train_data_dir,
    target_size=(img_width, img_height),
    batch_size=batch_size,
    class_mode='binary')

validation_generator = test_datagen.flow_from_directory(
    validation_data_dir,
    target_size=(img_width, img_height),
    batch_size=batch_size,
    class_mode='binary')

```
Finally model fitting:

```
model.fit_generator(
    train_generator,
    steps_per_epoch=nb_train_samples // batch_size,
    epochs=epochs,
    validation_data=validation_generator,
    validation_steps=nb_validation_samples // batch_size)
```
Saving the model:
```
model.save_weights('first_try.h5')
```
Step 5: Tesing the model

The challange for the readers is to use these weights, save the model as json and use that to run the model on sample image.

Where can I find it all?

You can go to my github repository where I have shared the exact same example with different classes (cats and dogs). The repo name is [babyStepsToMachineLearning](https://github.com/harshmunshi/babyStepsToMachineLearning)
