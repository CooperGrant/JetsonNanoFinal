# Construction-Tool-Identifier

This code is designed to identify the type of construction tool in a given image.

## The Algorithm

This project utilizes the jetson-inference library to identify the tools. This algorithm can recognize up to 1000 different classes of objects, but for this project, it has been specifically trained for the use of identifying construction tools like hammers and power drills.

![image of hammer that can be identified by this program](https://i.imgur.com/tJtDgS6.jpg)

![image of power drill that can also be identified by this program](https://i.imgur.com/SPPS46Z.jpg)


## Running this project

If you have not already intalled the jetson-inference library onto your jetson nano, follow these steps:


1. SSH into your nano, and create a new terminal.
2. Run these commands to make sure git and cmake are installed.

    sudo apt-get update
    sudo apt-get install git cmake
    
3. Clone the jetson-inference project

    git clone --recursive https://github.com/dusty-nv/jetson-inference
    cd jetson-inference
    git submodule update --init
    
4. In order to install the python packages, run this command.

    sudo apt-get install libpython3-dev python3-numpy
    
5. Next, make a build directory to build your project into. Make sure you are in the jetson-inference directory before running these commands.

    mkdir build
    
6. Change directories into build and run make to build the project.

    cd build
    cmake ../
    
7. Once this finishes loading you should see a space to run commands again. Make sure that you are still in the build directory and run these commands.

    make
    sudo make install
    sudo ldconfig




Once you have succesfully installed the jetson-inference library onto your nano, create a new file in your Construction_Tool_Identifier folder named Construction-Tool-Identifier.py, and enter this code to import the algorithm:

#!/usr/bin/python3

import jetson.inference
import jetson.utils

import argparse

parser = argparse.ArgumentParser()
parser.add_argument("filename", type=str, help="filename of the image to process")
parser.add_argument("--network", type=str, default="googlenet", help="model to use, can be:  googlenet, resnet-18, ect. (see --help for others)")
opt = parser.parse_args()

img = jetson.utils.loadImage(opt.filename)
net = jetson.inference.imageNet(opt.network)

class_idx, confidence = net.Classify(img)

class_desc = net.GetClassDesc(class_idx)
print("image is recognized as '{:s}' (class #{:d}) with {:f}% confidence".format(class_desc, class_idx, confidence * 100))



Once the algorithm and code have been completely set up, use these steps to run the code:

1. Import a JPEG image file of the construction tool you are looking to identify into the Construction-Tool-Identifier folder. 
2. Open a new terminal, and use this command to move into the correct folder.

    cd Construction-Tool-Identifier
    
3. To start the program, enter this command, entering the name of your JPEG file where it says [your JPEG file name here] (delete the brackets).

    python3 Construction-Tool-Identifier.py [your JPEG file name here]
    
4. The program will start running, and after a few seconds, will return what power tool your image contains.

[View a video explanation here](https://youtu.be/q9Mm9OMYNxY)
