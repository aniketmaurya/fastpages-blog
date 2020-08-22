---
title: Machine Learning APIs with FastAPI
description: Tutorial on FastAPI - high performance asynchronous framework for faster development of production ready APIs.
categories: [python]

toc: false
layout: post
badges: true
comments: true

image: https://fastapi.tiangolo.com/img/logo-margin/logo-teal.png


keywords: tensorflow, fastapi, python, web development, machine learning, computer vision
---

![](https://fastapi.tiangolo.com/img/logo-margin/logo-teal.png "Source- FastAPI Docs")

> FastAPI is a modern, fast (high-performance), web framework for building APIs with Python 3.6+ based on standard Python type hints.

FastAPI is a high performance asynchronous framework for building APIs in Python.
It provides support for Swagger UI out of the box.

Source code for this blog is available [aniketmaurya/tensorflow-fastapi-starter-pack](https://github.com/aniketmaurya/tensorflow-web-app-starter-pack)

### A simple hello-world example

First we import `FastAPI` class and create and object `app`. This class has useful [parameters](https://github.com/tiangolo/fastapi/blob/a6897963d5ff2c836313c3b69fc6062051c07a63/fastapi/applications.py#L30), like we can pass the title and description for Swagger UI.

```python
from fastapi import FastAPI
app = FastAPI(title='Hello world', description='This is a hello world example', version='0.0.1')
```

We define a function and decorate it with `@app.get`. This means that our api ``/index`` supports GET method. The function defined here is **async**, FastAPI automatically takes care of async and without async methods by creating a threadpool for the normal def functions and it uses async event loop for async functions.

```python
@app.get('/index')
async def hello_world():
    return "hello world"
```

### Image recognition API

We will create an API to classify images, we name it `predict/image`.
We will use Tensorflow for creating the image classification model.

> Tutorial for [Image Classification with Tensorflow](https://aniketmaurya.ml/blog/tensorflow/deep%20learning/2019/05/12/image-classification-with-tf2.html)

First we create a function `load_model`, which will return a MobileNet CNN Model with pretrained weights i.e. it is already trained to classify 1000 unique category of images.

```python
import tensorflow as tf

def load_model():
    model = tf.keras.applications.MobileNetV2(weights="imagenet")
    print("Model loaded")
    return model
```


```python
@app.post("/predict/image")
async def predict_api(file: UploadFile = File(...)):
    extension = file.filename.split(".")[-1] in ("jpg", "jpeg", "png")
    if not extension:
        return "Image must be jpg or png format!"
    image = read_imagefile(await file.read())
    prediction = predict(image)

    return prediction
```










<hr>
<br>
> Hope you liked the article.

👉 [Twitter](https://twitter.com/aniketmaurya): [https://twitter.com/aniketmaurya](https://twitter.com/aniketmaurya)

👉 Mail: aniketmaurya@outlook.com
