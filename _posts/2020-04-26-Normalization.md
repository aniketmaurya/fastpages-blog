---
toc: true
layout: post
description: Image Classification with Tensorflow 2.x.
categories: [Tensorflow, Deep Learning]
title: Normalization in Deep Learning

keywords: tensorflow, deep learning, image classification
badges: true
comments: true
image: 
layout: post
---

# Normalization in Deep Learning

Normalization is an important technique widely used in Deep Learning to achieve better results in less time.

### Why do we need to Normalize in the first place?

**Covariate Shift:** Most of the time the training dataset is very different from the real dataset. Suppose, a CNN model is trained to classify cats. But the training dataset only had images of black cats. So, if the model is fed with an image of a white cat it may not predict correctly.
This phenomenon is called **Covariate-Shift**.

In the graph below, the training data mostly falls on a linear function. But after including test data it looks more like a quadratic distribution.

![[Source](http://iwann.ugr.es/2011/pdf/InvitedTalk-FHerrera-IWANN11.pdf): [http://iwann.ugr.es/2011/pdf/InvitedTalk-FHerrera-IWANN11.pdf](http://iwann.ugr.es/2011/pdf/InvitedTalk-FHerrera-IWANN11.pdf)](https://cdn-images-1.medium.com/max/2000/0*y6ooHSrMwL6krpWY.png)*[Source](http://iwann.ugr.es/2011/pdf/InvitedTalk-FHerrera-IWANN11.pdf): [http://iwann.ugr.es/2011/pdf/InvitedTalk-FHerrera-IWANN11.pdf](http://iwann.ugr.es/2011/pdf/InvitedTalk-FHerrera-IWANN11.pdf)*

Generally in image datasets, the distribution can change because of change in camera resolutions or any other environmental change.
