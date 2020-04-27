---
date: 2018-03-05 00:00:00
title: "Reading Note: Neural Aesthetic Image Reviewer"
category: ["Computer Vision"]
tag: ["Reading Note", "Aesthetic Evaluation", "LSTM"]
---

**TITLE**: Tiny SSD: Neural Aesthetic Image Reviewer

**AUTHOR**: WenshanWang, Su Yang, Weishan Zhang, Jiulong Zhang

**ASSOCIATION**: Fudan University, China University of Petroleum, Xi’an University of Technology

**FROM**: [arXiv:1802.10240](https://arxiv.org/abs/1802.10240)

## CONTRIBUTION ##

1. The problem is whether computer vision systems can perceive image aesthetics as well as generate reviews or explanations as human. It is the first work to investigate into this problem.
2. By incorporating shared aesthetically semantic layers at a high level, an end-to-end trainable NAIR architecture is proposed, which can approach the goal of performing aesthetic prediction as well as generating natural-language comments related to aesthetics.
3. To enable this research, the AVA-Reviews dataset is collected, which contains 52,118 images and 312,708 comments. 

## METHOD ##

The framework of the work is illustrated in the following figure. The main idea of this work is to learn image aesthetic classification and vision-to-language generation using a multi-task framework.

![Framework](https://raw.githubusercontent.com/joshua19881228/my_blogs/master/Computer_Vision/Reading_Note/figures/Reading_Note_20180305_Framework.png "Framework")

The authors tried two designs, Model-I and Model-II. The difference between the two architectures is whether there are task-specific embedding layers for each task in addition to the shared layers. The potential limitation of Model-I is that some task-specific features can not be captured by the shared aesthetically semantic layer. Thus a task-specific embedding layer is introduced.

For image aesthetic classification part, it is a typical binary classification task. For comment generation part, LSTM is applied, the input of which is the high-level visual feature vector for an image.

### PERFORMANCE ###

![Performance](https://raw.githubusercontent.com/joshua19881228/my_blogs/master/Computer_Vision/Reading_Note/figures/Reading_Note_20180305_Performance.png "Performance")