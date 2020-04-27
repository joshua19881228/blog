---
date: 2016-08-30 00:00:00
title: "PVANET: Deep but Lightweight Neural Networks for Real-time Object Detection"
category: ["Computer Vision"]
tag: ["Reading Note", "Object Detection"]
---

**TITLE**: PVANET: Deep but Lightweight Neural Networks for Real-time Object Detection

**AUTHER**: Kye-Hyeon Kim, Yeongjae Cheon, Sanghoon Hong, Byungseok Roh, Minje Park

**ASSOCIATION**: Intel Imaging and Camera Technology

**FROM**: [arXiv:1608.08021](http://arxiv.org/abs/1608.08021)

### CONTRIBUTIONS ###

An efficient object detector based on CNN is proposed, which has the following advantages:

* Computational cost: 7.9GMAC for feature extraction with 1065x640 input (cf. ResNet-101: 80.5GMAC1)
* Runtime performance: 750ms/image (1.3FPS) on Intel i7-6700K CPU with a single core; 46ms/image (21.7FPS) on NVIDIA Titan X GPU
* Accuracy: 81.8% mAP on VOC-2007; 82.5% mAP on VOC-2012 (2nd place)

### Method ###

The author utilizes the pipline of Faster-RCNN, which is "CNN feature extraction + region proposal + RoI classification". The author claims that feature extraction part needs to be redesigned, since region proposal part is not computationally expensive and classification part can be efficiently compressed with common techniques like truncated SVD. And the principle is “less channels with more layers” and adoption of some building blocks including concatenated ReLU, Inception, and HyperNet. The structure of the network is as follows:

<img class="img-responsive center-block" src="https://raw.githubusercontent.com/joshua19881228/my_blogs/master/Computer_Vision/Reading_Note/figures/structure.jpg" alt="" width="640"/>

**Some Details**

1. Concatenated rectified linear unit (C.ReLU) is applied to the early stage of the CNNs (i.e., first several layers from the network input) to reduce the number of computations by half without losing accuracy. In my understanding, the C.ReLU encourages the network to learn Gabor-like filters and helps to accelerate the forward-propagation. If the output of the C.ReLu is 64, its convolution layer only needs 32-channel outputs. And it may harm the performance if it is used to the later stage of the CNNs, because it keeps the negative responses as activated signal, which means that a mad brain is trained.
2. Inception is applied to the remaining of the feature generation sub-network. An Inception module produces output activations of different sizes of receptive fields, so that increases the variety of receptive field sizes in the previous layer. All the design policies can be found in this [related work](http://joshua881228.webfactional.com/blog_reading-note-rethinking-the-inception-architecture-for-computer-vision_136/).
3. The author adopted the idea of multi-scale representation like HyperNet that combines several intermediate outputs so that multiple levels of details and non-linearities can be considered simultaneously. Direct concatenation of all abstraction layers may produce redundant information with much higher compute requirement and layers which are too early for object proposal and classification would be little help. The author combines 1) the last layer and 2) two intermediate layers whose scales are 2x and 4x of the last layer, respectively.
4. Residual structure is also used in this network, which helps to train very deep CNNs.