---
nav_exclude: true
layout: page
title: ModelVale
permalink: /ModelVale
---
## Table of contents
{: .no_toc .text-delta }
1. TOC
{:toc}

{% assign me = site.staffers | where: 'role', 'logo' %}
{% for staffer in me %}
{{ staffer }}
{% endfor %}

ModelVale is an app I designed, really as an excuse to tangentially work on machine learning
instead of app development while I was getting paid by Meta for the latter. The result is
something I am proud of.  

ModelVale is a unique game where machine learning comes to life. As you develop machine learning models, upload them to ModelVale! Each Model has a custom avatar and health. 
Train your Model and test on new data to earn more XP while developing robust neural networks! 
Over time, you can see your best performing models and their attributes, and keep training and testing their predictions to keep up their health.  
Features:
- Infinitely grow, train, and test your diverse world of Models! Level up XP over time  
- Train and share models with your friends and development team  
- Supports object detection, object classification, and more supervised, image-based architectures  
- Unprecedented access to out of distribution testing for image classifiers  
- Distributed, open-source ability to fine-tuning powerful models  
- Import custom Keras, PyTorch, CoreML, or CreateML models with Google Drive integration

### Code and Design
The app is written in Objective-C (to which I objected...) and runs natively on iOS platforms.
The code is currently open source and can be found [here](https://github.com/chaytanc/ModelVale).
Please feel free to add features! I would love to add support for non-image based models.
As a developer, I find it an incredibly useful tool to rapidly test out of distribution images
and quickly play with running and retraining models. Having the data flow at your fingertips
with instant results is pretty cool!

### Drive OAuth Data Usage
Because ModelVale integrates Google Drive to import new models, it requests access to view and 
download files from your Google Drive account. By leveraging Google Drive, a common tool to store
large files such as .mlmodel files, ModelVale can provide an easy way to transfer models developed
on more powerful devices to a mobile device. Then anyone can easily begin using their models on device for
fine-tuning and out of distribution tests!

The app does not download any files from Google Drive that are not downloaded by the user using the 
Import button. It does not view or edit any Drive files. It does not store any data from Google Drive
in databases. The downloaded files are stored locally in the Documents folder of the device they are 
downloaded on and are not used anywhere else.

[Privacy Policy](https://github.com/chaytanc/ModelVale/blob/main/PrivacyPolicy.md)


