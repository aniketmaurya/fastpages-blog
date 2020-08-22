---
title: Building Machine Learning API with FastAPI
description: Tutorial on FastAPI - high performance asynchronous framework for faster development of production ready APIs.
categories: [python]

toc: true
layout: post
badges: true
comments: true

image: https://fastapi.tiangolo.com/img/logo-margin/logo-teal.png


keywords: tensorflow, fastapi, python, web development, machine learning, computer vision
---

![](https://fastapi.tiangolo.com/img/logo-margin/logo-teal.png "Source- FastAPI Docs")


FastAPI is a high performance asynchronous framework for building APIs in Python.
It provides support for Swagger UI out of the box.

> Source code for this blog is available [aniketmaurya/tensorflow-fastapi-starter-pack](https://github.com/aniketmaurya/tensorflow-web-app-starter-pack)

# Lets start with a simple hello-world example

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


# Pydantic support

One of my favourite feature offered by FastAPI is Pydantic support. We can define Pydantic models and request response will be handled by FastAPI for these models.
Lets create a covid-19 symptom checker API to understand this.

## Covid-19 symptom checker API
We create request body, it is the format in which client should send the request.

```python
from pydantic import BaseModel

class Symptom(BaseModel):
    fever: bool = False
    dry_cough: bool = False
    tiredness: bool = False
    breathing_problem: bool = False
```

Let's create a function to assign risk level based on the inputs.

> This is just for learning and should not be used in real life, better consult a doctor.

```python
def get_risk_level(symptom: Symptom):
    if not (symptom.fever or symptom.dry_cough or symptom.tiredness or symptom.breathing_problem):
        return 'Low risk level. THIS IS A DEMO APP'
    
    if not (symptom.breathing_problem or symptom.dry_cough):
        if symptom.fever:
            return 'moderate risk level. THIS IS A DEMO APP'
    
    if symptom.breathing_problem:
        return 'High risk level. THIS IS A DEMO APP'
    
    return 'THIS IS A DEMO APP'

```

Lets create the API for checking the symptoms

```python
@app.post('/api/covid-symptom-check')
def check_risk(symptom: Symptom):
    return get_risk_level(symptom)
```


# Image recognition API

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

Now we will create an API `/predict/image` which supports file upload. We will filter the file extension to support only jpg, jpeg and png format of images.

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


# Final code

```python
import uvicorn
from fastapi import FastAPI, File, UploadFile

from application.components import predict, read_imagefile

app = FastAPI()

@app.post("/predict/image")
async def predict_api(file: UploadFile = File(...)):
    extension = file.filename.split(".")[-1] in ("jpg", "jpeg", "png")
    if not extension:
        return "Image must be jpg or png format!"
    image = read_imagefile(await file.read())
    prediction = predict(image)

    return prediction


@app.post("/api/covid-symptom-check")
def check_risk(symptom: Symptom):
    return symptom_check.get_risk_level(symptom)


if __name__ == "__main__":
    uvicorn.run(app, debug=True)

```

<hr>
<br>
> Hope you liked the article.

👉 Twitter: [https://twitter.com/aniketmaurya](https://twitter.com/aniketmaurya)

👉 Linkedin: [https://linkedin.com/in/aniketmaurya](https://linkedin.com/in/aniketmaurya)