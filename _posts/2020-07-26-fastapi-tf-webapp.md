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

### A simple hello-world example

We first import fastapi `FastAPI` class and create and object. This object have many useful (parameters)[https://github.com/tiangolo/fastapi/blob/a6897963d5ff2c836313c3b69fc6062051c07a63/fastapi/applications.py#L30]. We can pass the title and description for Swagger UI.

```python
from fastapi import FastAPI
app = FastAPI(title='Hello world', description='This is a hello world example', version='0.0.1')
```


```python
@app.get('/)
async def hello_world():
    return "hello world"
```

### Image recognition API
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
