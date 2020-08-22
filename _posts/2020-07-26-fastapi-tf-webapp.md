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

model = load_model()
```

We define a `predict` function that will accepts an image and returns the predictions.
We resize the image to 224x224 and normalize the pixel values to be in **[-1, 1]**.

```python
from tensorflow.keras.applications.imagenet_utils import decode_predictions
```
`decode_predictions` is used to decode class name of the predicted object. 
Here we will return top-2 probable class.

```python
def predict(image: Image.Image):

    image = np.asarray(image.resize((224, 224)))[..., :3]
    image = np.expand_dims(image, 0)
    image = image / 127.5 - 1.0

    result = decode_predictions(model.predict(image), 2)[0]

    response = []
    for i, res in enumerate(result):
        resp = {}
        resp["class"] = res[1]
        resp["confidence"] = f"{res[2]*100:0.2f} %"

        response.append(resp)

    return response
```

Now we will create an api `/predict/image` which supports file upload. We will filter the file extension to support only jpg, jpeg and png format of images.

We will use Pillow to load the uploaded image.

```python
def read_imagefile(file) -> Image.Image:
    image = Image.open(BytesIO(file))
    return image
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

Finally our code will look like this -

```python
import uvicorn
from fastapi import FastAPI, File, UploadFile
from starlette.responses import RedirectResponse

from application.components import predict, read_imagefile

app = FastAPI(title='Tensorflow Web app Starter Pack', description='<h2>Try this app by uploading any image with `predict/image`</h2><br>by Aniket Maurya')


@app.get('/', include_in_schema=False)
async def index():
    return RedirectResponse(url='/docs')


@app.post("/predict/image")
async def predict_api(file: UploadFile = File(...)):
    extension = file.filename.split(".")[-1] in ("jpg", "jpeg", "png")
    if not extension:
        return "Image must be jpg or png format!"
    image = read_imagefile(await file.read())
    prediction = predict(image)

    return prediction


if __name__ == "__main__":
    uvicorn.run(app, debug=True)
```


<hr>
<br>
> Hope you liked the article.

👉 [Twitter](https://twitter.com/aniketmaurya): [https://twitter.com/aniketmaurya](https://twitter.com/aniketmaurya)

👉 [Linkedin](https://linkedin.com/in/aniketmaurya): [https://linkedin.com/in/aniketmaurya](https://linkedin.com/in/aniketmaurya)

👉 Mail: aniketmaurya@outlook.com
