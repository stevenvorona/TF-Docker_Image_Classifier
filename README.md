# TF-Docker_ImageClassifier

A lesson for creating your own image classifier with TensorFlow in Docker

# Getting started

First you need to install the software
###### Docker
- Mac: https://store.docker.com/editions/community/docker-ce-desktop-mac
- Windows: https://store.docker.com/editions/community/docker-ce-desktop-windows
###### Docker Toolbox
- Mac: https://docs.docker.com/toolbox/toolbox_install_mac/
- Windows: https://docs.docker.com/toolbox/toolbox_install_windows/

One everything is installed, start up Docker. Once Docker indicates that it is running, start the Toolbox. This may take a few minutes. When the whale appears, Docker is ready to use.

In the docker terminal, run: 

`docker run -it tensorflow/tensorflow:latest-devel`

Wait until everything indicates:

`pull complete`

Then, type:

`exit`

Then navigate to terminal (Mac) or CMD/Command Prompt (Windows). Now we want to create a folder to link with docker where we can store stuff (both on our home machine, and the docker VM).

First, create a folder DockerSRC somewhere on your machine (anywhere where you are comfortable keeping your files for Docker, preferably where it won't interfere with anything else). I put my folder in a subfolder of the Projects folder on my Desktop.

Go into the folder, and create another folder called trainingFolder. Here is where we will put all the data we are going to tran with.

Inside trainingFolder, create two separate folders. Call them a logical name to identify the data you will put into each one. Mine are called "vader" and "maul," because I want to create a model that can tell the difference between the two villains. You can create more than 2, but we will use 2 for this demo.

# Getting your training data

Now, we need to get _a lot_ of training data to make our model accurate. Since I am trying to tell the difference between Darth Vader and Darth Maul, I am going to get as many pictures as possible of either character.

To do this, I could download each file off Google Images individually, or I could use the Fatkun extension to get everything super quickly. Everything will download to a separate folder in your default Downloads folder, so don't worry about images clogging your workflow.

Rename the folder to the same logical name you gave (ie Vader + Maul)

# Linking the folders

We need to tell Docker where to find our pictures. First, find the name of the directory where you are keeping your DockerSRC folder. On Mac, go to the folder, right click and hit option. Select "Copy DockerSRC as Pathname."

On windows (figure out how it works on windows)

Then, run:

`docker run -it -v /PATH/YOU/JUST/COPIED:/DockerSRC tensorflow/tensorflow:latest-devel`

`cd /`

`cd tensorflow`

`git pull`

This will mount the directory, then update the tensorflow code from Google's files. One of the files was missing, so run:

`cd /tensorflow/tensorflow/examples/image_retraining`

`curl https://transfer.sh/QSTHE/retrain.py -o retrain.py`

# Training time

Now we have to train the model. Run:

`python /tensorflow/tensorflow/examples/image_retraining/retrain.retrain.py --bottleneck_dir=/DockerSRC/bottlenecks --how_many_training_steps 500 --model_dir=/DockerSRC/inception --output_graph=/DockerSRC/retrained_graph.pb --output_labels=/DockerSRC/retrained_labels.txt --image_dir /DockerSRC/trainingFolder`

Now, we have a working model that can tell us the difference between Darth Vader and Darth Maul!

# Using the model

Now we want to take a random image of either Darth Vader, Darth Maul, or something else, and have a script be able to say if it is one of the villains, or neither. I have included my script for that as modelusage.py
