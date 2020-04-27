---
date: 2019-01-20 00:00:00
title: "Reading Note: CornerNet: Detecting Objects as Paired Keypoints"
category: ["Computer Vision"]
tag: ["Reading Note", "CNN", "Object Detection"]
---

**TITLE**: CornerNet: Detecting Objects as Paired Keypoints

**AUTHOR**: Hei Law, Jia Deng

**ASSOCIATION**: University of Michigan

**FROM**: [arXiv:1808.01244](https://arxiv.org/abs/1808.01244)

## CONTRIBUTION

1. CornerNet, a new one-stage approach to object detection is introduced that does without anchor boxes. We detect an object as a pair of
2. a new type of pooling layer, _coner pooling_, that helps a convolutional network better localize corners of bounding boxes is introduced.

## METHOD

### Motivation

The use of anchor boxes has two drawbacks.

First, a very large set of anchor boxes are of need, e.g. more than 40k in DSSD and more than 100k in RetinaNet. This is because the detector is trained to classify whether each anchor box sufficiently overlaps with a ground truth box, and a large number of anchor boxes is needed to ensure sufficient overlap with most ground truth boxes, leading to huge imbalance between positive and negative anchor boxes.

Second, the use of anchor boxes introduces many hyperparameters and design choices. These include how many boxes, what sizes, and what aspect ratios. Such choices have largely been made via ad-hoc heuristics, and can become even more complicated when combined with multiscale architectures.

### DetNet Design

In CornerNet, an object is detected as a pair of keypoints, the top-left corner and bottom-right corner of the bounding box. A convolutional network predicts two sets of heatmaps to represent the locations of corners of different object categories, one set for the top-left corners and the other for the bottom-right corners. The network also predicts an embedding vector for each detected corner such that the distance between the embeddings of two corners from the same object is small. To produce tighter bounding boxes, the network also predicts offsets to slightly adjust the locations of the corners. The framework is illustrated in the following figure.

![Framework](/img/ReadingNote/20190120/framework.png "Framework")

To generate heatmaps, embeddings and offsets, the following network structure is utilized. The heatmaps' goal is detecting corners. The embeddings are used to group corners that belong to the same object. Conner pooling is used in the network to extract features for corner detection.

![Framework](/img/ReadingNote/20190120/network-block.png "network structure")

The annotation of heatmaps is generated in a way that similar to that in detection joints in pose estimation methods. Embeddings are generated within pairs that generated from corners. The offsets are regressed from the difference between actual division of coordinates and downsampling ratio and the floored division.

The backbone of the network is two hourglasses.

## PERFORMANCE

![Performance](/img/ReadingNote/20190120/performance.png "Performance")

## SOME THOUGHTs

1. How to select numbers of hourglasses?
2. The speed of the method is slow. The average inference time is 244ms per image on a Titan X.
3. The key of the method is detecting corners and more work can be done in this task.
