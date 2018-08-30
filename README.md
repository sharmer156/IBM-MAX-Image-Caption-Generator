[![Build Status](https://travis-ci.org/IBM/MAX-Image-Caption-Generator.svg?branch=master)](https://travis-ci.org/IBM/MAX-Image-Caption-Generator)

# IBM Code Model Asset Exchange: Show and Tell Image Caption Generator
WEB APP connect https://github.com/IBM/MAX-Image-Caption-Generator-Web-App/

![](https://github.com/IBM/MAX-Image-Caption-Generator-Web-App/blob/master/doc/source/images/webui.png)
 

**CONTENT** 

* [IBM Code Model Asset Exchange: Show and Tell Image Caption Generator](#ibm-code-model-asset-exchange-show-and-tell-image-caption-generator)
	* [easy start](#easy-start)
	* [1 windows error](#1-windows-error)
	* [2 ubuntu BUG](#2-ubuntu-bug)
	* [bug1 Could not get lock](#bug1-could-not-get-lock)
	* [bug2 URL connect erro(404?)](#bug2-url-connect-erro404)
	* [Model Metadata](#model-metadata)
	* [References](#references)
	* [Licenses](#licenses)
	* [Pre-requisites:](#pre-requisites)
* [Steps](#steps)
	* [Deploy from Docker Hub](#deploy-from-docker-hub)
	* [Deploy on Kubernetes](#deploy-on-kubernetes)
	* [Run Locally](#run-locally)
	* [Links](#links)

## easy start
** $ git clone -->$ cd-->$ docker build-->$ docker run **
### mian comande
```
$ git clone https://github.com/IBM/MAX-Image-Caption-Generator.git
$ cd MAX-Image-Caption-Generator
docker build -t max-im2txt  .                //max-im2txt  .两个空格不可少
docker run -it -p 5000:5000 max-im2txt       //BUG:does not exist or no pull access.需要
docker build -t max-image-caption-generator .
docker run -it -p 5000:5000 max-image-caption-generator //WARNING: Do not use the development server in a production environment.

```
see the datial in [Terminal.txt](#Terminal.txt)

-  docker build max-im2txt  . 
contait 10 Steps 
<!--auto run--> 

```
Step 1/10 : FROM codait/max-base
Step 2/10 : ARG model_bucket=http://max-assets.s3-api.us-geo.objectstorage.softlayer.net/tf/im2txt
Step 3/10 : ARG model_file=im2txt_ckpt.tar.gz
Step 4/10 : WORKDIR /workspace
Step 5/10 : RUN wget -nv ${model_bucket}/${model_file} --output-document=/workspace/assets/${model_file}
Step 6/10 : RUN tar -x -C assets/ -f assets/${model_file} -v
Step 7/10 : RUN pip install tensorflow &&     pip install Pillow
Step 8/10 : COPY . /workspace
Step 9/10 : EXPOSE 5000
Step 10/10 : CMD python app.py

```
- docker build -t max-image-caption-generator .
also contait 10 Steps
<!--auto run--> 

```
Step 1/10 : FROM codait/max-base
Step 2/10 : ARG model_bucket=http://max-assets.s3-api.us-geo.objectstorage.softlayer.net/tf/im2txt
Step 3/10 : ARG model_file=im2txt_ckpt.tar.gz
Step 4/10 : WORKDIR /workspace
Step 5/10 : RUN wget -nv ${model_bucket}/${model_file} --output-document=/workspace/assets/${model_file}
Step 6/10 : RUN tar -x -C assets/ -f assets/${model_file} -v
Step 7/10 : RUN pip install tensorflow &&     pip install Pillow
Step 8/10 : COPY . /workspace
Step 9/10 : EXPOSE 5000
Step 10/10 : CMD python app.py

```
## 1 windows error

```
docker build -t max-im2txt  .
Sending build context to Docker daemon  963.1kB
Step 1/10 : FROM codait/max-base
latest: Pulling from codait/max-base
image operating system "linux" cannot be used on this platform
```
so it can't be run in the windows

## 2 ubuntu BUG
## bug1 Could not get lock

```
sudo apt install docker.io
```
```
E: Could not get lock /var/lib/dpkg/lock - open (11: Resource temporarily unavailable)
E: Unable to lock the administration directory (/var/lib/dpkg/), is another process using it?
```
** fix it **
https://blog.csdn.net/kevin_android_123456/article/details/8174343

```
sudo rm /var/cache/apt/archives/lock
sudo rm /var/lib/dpkg/lock
```
## bug2 URL connect erro(404?)
run
```
docker build -t max-im2txt  .

```
but can't connect the this URL
```
2018-08-30 05:53:43 URL:http://max-assets.s3-api.us-geo.objectstorage.softlayer.net/tf/im2txt/im2txt_ckpt.tar.gz [276638750/276638750] -> "/workspace/assets/im2txt_ckpt.tar.gz" [1]

```
```
Retrying (Retry(total=4, connect=None, read=None, redirect=None, status=None)) after connection broken by 'ReadTimeoutError("HTTPSConnectionPool(host='pypi.org', port=443): Read timed out. (read timeout=15)",)': /simple/numpy/

```
img
![](example.jpeg)
img
```
{
  "status": "ok",
  "predictions": [
    {
      "index": "0",
      "caption": "a man in a suit and tie standing in front of a building .",
      "probability": 0.0005420378725991916
    },
    {
      "index": "1",
      "caption": "a man in a suit and tie standing next to a building .",
      "probability": 0.00014754194193780548
    },
    {
      "index": "2",
      "caption": "a man in a suit and tie standing in a street .",
      "probability": 0.00013819170399036678
    }
  ]
}

```

This repository contains code to instantiate and deploy an image caption generation model. This model generates captions from a fixed vocabulary that describe the contents of images in the [COCO Dataset](http://cocodataset.org/#home). The model consists of an _encoder_ model - a deep convolutional net using the Inception-v3 architecture trained on [ImageNet-2012 data](http://www.image-net.org/challenges/LSVRC/2012/) - and a _decoder_ model - an LSTM network that is trained conditioned on the encoding from the image _encoder_ model. The input to the model is an image, and the output is a sentence describing the image content.

The model is based on the [Show and Tell Image Caption Generator Model](https://github.com/tensorflow/models/tree/master/research/im2txt). The checkpoint files are hosted on [IBM Cloud Object Storage](http://max-assets.s3-api.us-geo.objectstorage.softlayer.net/tf/im2txt/im2txt_ckpt.tar.gz). The code in this repository deploys the model as a web service in a Docker container. This repository was developed as part of the [IBM Code Model Asset Exchange](https://developer.ibm.com/code/exchanges/models/).

## Model Metadata
| Domain | Application | Industry  | Framework | Training Data | Input Data Format |
| ------------- | --------  | -------- | --------- | --------- | -------------- | 
| Vision | Image Caption Generator | General | TensorFlow | [COCO](http://cocodataset.org/#home) | Images | 

## References
* _O. Vinyals, A. Toshev, S. Bengio, D. Erhan._, ["Show and Tell: Lessons learned from the 2015 MSCOCO Image Captioning Challenge"](https://arxiv.org/abs/1609.06647), IEEE transactions on Pattern Analysis and Machine Intelligence, 2016.
* [im2txt TensorFlow Model GitHub Page](https://github.com/tensorflow/models/tree/master/research/im2txt)
* [COCO Dataset Project Page](http://cocodataset.org/#home)

## Licenses

| Component | License | Link  |
| ------------- | --------  | -------- |
| This repository | [Apache 2.0](https://www.apache.org/licenses/LICENSE-2.0) | [LICENSE](LICENSE) |
| Model Weights | [MIT](https://opensource.org/licenses/MIT) | [Pretrained Show and Tell Model](https://github.com/KranthiGV/Pretrained-Show-and-Tell-model) |
| Model Code (3rd party) | [Apache 2.0](https://www.apache.org/licenses/LICENSE-2.0) | [im2txt](https://github.com/tensorflow/models/tree/master/research/im2txt) |
| Test assets | Various | [Asset README](assets/README.md) |

## Pre-requisites:

* `docker`: The [Docker](https://www.docker.com/) command-line interface. Follow the [installation instructions](https://docs.docker.com/install/) for your system.
* The minimum recommended resources for this model is 2GB Memory and 2 CPUs.

# Steps

1. [Deploy from Docker Hub](#deploy-from-docker-hub)
2. [Deploy on Kubernetes](#deploy-on-kubernetes)
3. [Run Locally](#run-locally)

## Deploy from Docker Hub

To run the docker image, which automatically starts the model serving API, run:

```
$ docker run -it -p 5000:5000 codait/max-image-caption-generator
```

This will pull a pre-built image from Docker Hub (or use an existing image if already cached locally) and run it.
If you'd rather checkout and build the model locally you can follow the [run locally](#run-locally) steps below.

## Deploy on Kubernetes

You can also deploy the model on Kubernetes using the latest docker image on Docker Hub.

On your Kubernetes cluster, run the following commands:

```
$ kubectl apply -f https://raw.githubusercontent.com/IBM/MAX-Image-Caption-Generator/master/max-image-caption-generator.yaml
```

The model will be available internally at port `5000`, but can also be accessed externally through the `NodePort`.

## Run Locally

1. [Build the Model](#1-build-the-model)
2. [Deploy the Model](#2-deploy-the-model)
3. [Use the Model](#3-use-the-model)
4. [Development](#4-development)
5. [Cleanup](#5-cleanup)

### 1. Build the Model

Clone this repository locally. In a terminal, run the following command:

```
$ git clone https://github.com/IBM/MAX-Image-Caption-Generator.git
```

Change directory into the repository base folder:

```
$ cd MAX-Image-Caption-Generator
```

To build the docker image locally, run: 

```
$ docker build -t max-image-caption-generator .
```

All required model assets will be downloaded during the build process. _Note_ that currently this docker image is CPU only (we will add support for GPU images later).

### 2. Deploy the Model

To run the docker image, which automatically starts the model serving API, run:

```
$ docker run -it -p 5000:5000 max-image-caption-generator
```

### 3. Use the Model

The API server automatically generates an interactive Swagger documentation page. Go to `http://localhost:5000` to load it. From there you can explore the API and also create test requests.

Use the `model/predict` endpoint to load a test file and get captions for the image from the API.

![pic](/docs/swagger-screenshot.png "Swagger Screenshot")

You can also test it on the command line, for example:

```
$ curl -F "image=@assets/surfing.jpg" -X POST http://localhost:5000/model/predict
```

```json
{
  "status": "ok",
  "predictions": [
    {
      "index": "0",
      "caption": "a man riding a wave on top of a surfboard .",
      "probability": 0.038827644239537
    },
    {
      "index": "1",
      "caption": "a person riding a surf board on a wave",
      "probability": 0.017933410519265
    },
    {
      "index": "2",
      "caption": "a man riding a wave on a surfboard in the ocean .",
      "probability": 0.0056628732021868
    }
  ]
}
```

### 4. Development

To run the Flask API app in debug mode, edit `config.py` to set `DEBUG = True` under the application settings. You will then need to rebuild the docker image (see [step 1](#1-build-the-model)).

### 5. Cleanup

To stop the Docker container, type `CTRL` + `C` in your terminal.

## Links

* [Image Caption Generator Web App](http://developer.ibm.com/code/patterns/create-web-app-interact-machine-learning-generated-image-captions): A reference application created by the IBM CODAIT team that uses the Image Caption Generator
