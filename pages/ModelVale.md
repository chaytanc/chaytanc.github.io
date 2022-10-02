---
nav_exclude: true
layout: page
title: ModelVale
permalink: /ModelVale
---
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

[Privacy Policy](https://github.com/chaytanc/ModelVale/blob/main/PrivacyPolicy.md)


