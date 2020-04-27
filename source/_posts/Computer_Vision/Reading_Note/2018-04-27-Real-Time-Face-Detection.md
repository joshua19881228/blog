---
date: 2018-04-27 00:00:00
title: "Reading Note: Real-Time Rotation-Invariant Face Detection with Progressive Calibration Networks"
category: ["Computer Vision"]
tag: ["Reading Note", "CNN", "Real-time", "Face Detection"]
---

**TITLE**: Real-Time Rotation-Invariant Face Detection with Progressive Calibration Networks

**AUTHOR**: Xuepeng Shi, Shiguang Shan, Meina Kan, Shuzhe Wu, Xilin Chen

**ASSOCIATION**: Chinese Academy of Sciences

**FROM**: [arXiv:1804.06039](https://arxiv.org/abs/1804.06039)

## CONTRIBUTION ##

1. A real-time and accurate rotation-invariant face detector with progressive calibration networks (PCN) is proposed.
2. PCN divides the calibration process into several progressive steps, each of which is an easy task, rsulting in accurate calibration with low time cost. And the range of full rotation-in-plane (RIP) angles is gradually decreasing, which helps distinguish faces from non-faces.
3. In the first two stages of PCN, only coarse calibrations are conducted, such as calibrations from facing down to facing up, and from facing left to facing right. On the one hand, a robust and accurate RIP angle prediction for this coarse calibration is easier to attain without extra time cost, by jointly learning calibration task with the classification task and bounding box regression task in a multi-task learning manner. On the other hand, the calibration can be easier to implement as flipping original image with quite low time cost.

## METHOD ##

### Framework ###

Given an image, all face candidates are obtained according to the sliding window and image pyramid principle, and each candidate window goes through the detector stage by stage. In each stage of PCN, the detector simultaneously rejects most candidates with low face confidences, regresses the bounding boxes of remaining face candidates, and calibrates the RIP orientations of the face candidates. After each stage, non-maximum suppression (NMS) is used to merge those highly overlapped candidates.

PCN progressively calibrates the RIP orientation of each face candidate to upright for better distinguishing faces from non-faces. 

1. PCN-1 first identifies face candidates and calibrates those facing down to facing up, halving the range of RIP angles from [$-180^{\circ}$,$180^{\circ}$] to [$-90^{\circ}$, $90^{\circ}$]. 
2. Then the rotated face candidates are further distinguished and calibrated to an upright range of [$-45^{\circ}$, $45^{\circ}$] in PCN-2, shrinking the RIP ranges by half again. 
3. Finally, PCN-3 makes the accurate final decision for each face candidate to determine whether it is a face and predict the precise RIP angle. Briefly,

The following figure illustrates the framework.

![Framework](https://raw.githubusercontent.com/joshua19881228/my_blogs/master/Computer_Vision/Reading_Note/figures/Reading_Note_20180427_Framework.png "Framework")

### First Stage PCN-1 ###

For each input window $x$, PCN-1 has three objectives: face or non-face classification, bounding box regression, and calibration, formulated as follows:

$$[f, t, g] = F_{1}(x)$$

where $F_{1}$ is the detector in the first stage structured with a small CNN. The $f$ is face confidence score, $t$ is a vector representing the prediction of bounding box regression, and $g$ is orientation score. Overall, the objective for PCN-1 in the first stage is defined as:

$$\min L = L_{cls} +\lambda_{reg} \cdot L_{reg} + \lambda_{cal} \cdot L_{cal}$$

where $\lambda_{reg}$, $\lambda_{cal}$ are parameters to balance different loss. The first objective, which is also the primary objective, aims for distinguishing faces from non-faces. The second objective attempts to regress the fine bounding box. The third objective aims to predict the coarse orientation of the face candidate in a binary classification manner, telling the candidate is facing up or facing down.

The PCN-1 can be used to filter all windows to get a small number of face candidates. For the remaining face candidates, firstly they are updated to the new regressed bounding boxes. Then the updated face candidates are rotated according to the predicted coarse RIP angles.

### Second Stage PCN-2 ###

Similar as the PCN-1 in the first stage, the PCN-2 in the second stage further distinguishes the faces from non-faces more accurately, regresses the bounding boxes, and calibrates face candidates. Differently, the coarse orientation prediction in this stage is a ternary classification of the RIP angle range, telling the candidate is facing left, right or front.

### Third Stage PCN-3 ###

After the second stage, all the face candidates are calibrated to an upright quarter of RIP angle range, i.e. [$-45^{\circ}$,$45^{\circ}$]. Therefore, the PCN-3 in the third stage can easily and accurately determine whether it is a face and regress the bounding box. Since the RIP angle has been reduced to a small range in previous stages, PCN-3 attempts to directly regress the precise RIP angles of face candidates instead of coarse orientations.

### Accurate and Fast Calibration ###

The early stages only predict coarse RIP ori- entations, which is robust to the large diversity and further benefits the prediction of successive stages.

The calibration based on the coarse RIP prediction can be efficiently achieved via flipping original image three times, which brings almost no additional time cost. Rotating the original image by $-90^{\circ}$, $90^{\circ}$ and $180^{\circ}$ to get image-left, image-right, and image-down. And the windows with $0^{\circ}$,$-90^{\circ}$, $90^{\circ}$ and $180^{\circ}$ can be cropped from original image, image-left, image-right, and image-down respectively, as the following figure shows.

![Calibration](https://raw.githubusercontent.com/joshua19881228/my_blogs/master/Computer_Vision/Reading_Note/figures/Reading_Note_20180427_Calibration.png "Calibration")

### CNN Architecture ###

![CNN Architecture](https://raw.githubusercontent.com/joshua19881228/my_blogs/master/Computer_Vision/Reading_Note/figures/Reading_Note_20180427_CNN.png "CNN Architecture")

## PERFORMANCE ##

![Performance](https://raw.githubusercontent.com/joshua19881228/my_blogs/master/Computer_Vision/Reading_Note/figures/Reading_Note_20180427_Performance.png "Performance")