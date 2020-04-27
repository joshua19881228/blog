---
date: 2015-11-06 00:00:00
title: "READING NOTE: Two-Stream Convolutional Networks for Action Recognition in Videos"
category: ["Computer Vision"]
tag: ["Reading Note", "Action Recognition", "CNN"]
---

**TITLE**: Two-Stream Convolutional Networks for Action Recognition in Videos

**AUTHOR**: Simonyan, Karen and Zisserman, Andrew

**FROM**: NIPS2014

### CONTRIBUTIONS ###

1. A two-stream ConvNet combines spatial and temporal networks.
2. A ConvNet trained on multi-frame dense optical flow is able to achieve a good performance in spite of small training dataset
3. Multi-task training procedure benefits performance on different datasets.

### METHOD ###

Two-stream architecture convolutional network:
1. Spatial stream ConvNet: take a still frame as input and perform action recognition in this single frame.
2. Temporal stream ConvNet: take a 2L-channel optical flow/trajectory stacking corresponding to the still frame as input and perform action recognition in this multi-channel input.
3. The two outputs of the streams are concated as a feature to train a SVM classifier to fuse them.

<img class="img-responsive center-block" src="https://raw.githubusercontent.com/joshua19881228/my_blogs/master/Computer_Vision/Reading_Note/figures/two-stream.png" alt="" width="640"/>

**SOME DETAILS**
    
1. Mean flow subtraction is utilized to eliminate displacements caused by camera movement.
2. At test stage, 25 frames (time points) are extracted and their corresponding 2L-channel stackings are sent to the network. In addition, 5 patches and their flips are extracted in space domain.

### ADVANTAGES ###

1. Simulate bio-structure of human visual cortex.
2. Competitive performance with the state of the art representations in spite of small size of training dataset.
3. CNN with convolution filters could generalize hand-crafted features.

### DISADVANTAGES ###

1. Can not localize action in neither spatial nor temporal domain.