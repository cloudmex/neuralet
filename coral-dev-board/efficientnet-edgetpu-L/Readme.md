# Coral efficientnet-edgetpu-L model:
This is a dockerized implementation of the [Coral](https://coral.ai/) Efficientnet large for google's [Coral Dev Board](https://coral.ai/products/dev-board/) devices. [Link to the original model.](https://github.com/google-coral/edgetpu/raw/master/test_data/efficientnet-edgetpu-L_quant_edgetpu.tflite)

Efficientnet-L (large size) is a member of efficientnet family introduced by Mingxing Tan and Quoc V. Le in the paper [EfficientNet: Rethinking Mo del Scaling for Convolutional Neural Networks](https://arxiv.org/abs/1905.11946), which achieve state-of-the-art accuracy, yet being an order-of-magnitude smaller and faster than previous models.

This classification model is trained on [ImageNet](http://www.image-net.org/) dataset and can recognize 1000 different object categories. The input image size of the network must be ```300x300```.

# Codebase architecture:
The ```server-example.py``` is responsible to run inference on the input image and put the result in the output queue. The server will automatically download the edge-TPU compiled tflite model from [neuralet](https://github.com/neuralet/neuralet-models) if it doesn't exist under ```data/models```. The server will start automatically when the container starts.

The ```client.py``` contains a simple script that allows user to prepare their input data and push it to the queue. The input of the server should be a RGB image with the shape of ```(300,300,3)```.

# Getting started:
There are two main ways to run this container. You can build the container from the Dockerfile or pull it from the Dockerhub.
## Build container from source:

```
# 1- Clone the repository
git clone https://github.com/neuralet/neuralet

# 2- Build the container
MODEL_NAME=efficientnet-edgetpu-L
cd neuralet/coral-dev-board/$MODEL_NAME
docker build -t "neuralet/coral-dev-board:$MODEL_NAME" .

# 3- Run the container
docker run -it --privileged --net=host -v $(pwd)/../../:/repo neuralet/coral-dev-board:$MODEL_NAME

# 4- Run inference on a test image
python3 src/client.py [PATH-TO-IMAGE]

# 5- Terminate the server and stop the container
python3 src/client.py stop
```

## Pull container from Dockerhub:

```
# 1- Clone the repository
git clone https://github.com/neuralet/neuralet

# 2- Run the container
MODEL_NAME=efficientnet-edgetpu-L
cd neuralet/coral-dev-board/$MODEL_NAME
docker run -it --privileged --net=host -v $(pwd)/../../:/repo neuralet/coral-dev-board:$MODEL_NAME

# 3- Run inference on a test image
python3 src/client.py [PATH-TO-IMAGE]

# 4- Terminate the server and stop the container
python3 src/client.py stop
```
