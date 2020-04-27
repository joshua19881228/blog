---
date: 2015-11-05 00:00:00
title: "READING NOTE: READING NOTE: Joint Tracking and Segmentation of Multiple Targets"
category: ["Computer Vision"]
tag: ["Reading Note", "Segmentation", "Tracking"]
---

**TITLE**: Joint Tracking and Segmentation of Multiple Targets

**AUTHOR**: Milan, Anton and Leal-Taixe, Laura and Schindler, Konrad and Reid, Ian

**FROM**: CVPR2015

### CONTRIBUTIONS ###

1. A new CRF model taking advantage of both high-level detector responses and low-level superpixel information
2. Fully automated segmentation and tracking of an unknown number of targets.
3. A complete state representation at every time step could handle occlusions

### METHOD ###

1. Generate an overcomplete set of trajectory hypotheses.
2. Solve data association problem by optimizing an objective function, which is a multi-label conditional random field (CRF).

#### SOME DETAILS ####
    
<img class="img-responsive center-block" src="https://raw.githubusercontent.com/joshua19881228/my_blogs/master/Computer_Vision/Reading_Note/figures/MRF.png" alt="" width="480"/>
    
The goal is to find the most probable labeling for all nodes given the observations, which is equivalent to
    
$$ v^{*} = \mathrm{argmin_{v}}E(\nu) $$
    
in which
    
$$ E(\nu) = \sum_{s\in\large{\nu}_{S}}\phi^{\large{\nu}_{S}}(s) + \sum_{d\in\large{\nu}_{D}}\phi^{\large{\nu}_{D}}(d) + \sum_{(v,w)\in\Large{\varepsilon}}\psi(v,w)+\psi^{\lambda}$$
    
where \\(\phi^{\large{\nu}_{S}}\\) and \\(\phi^{\large{\nu}_{D}}\\) are unary potential functions for superpixel and detection nodes, respectively, measuring the cost of one detection node in \\(\large{\nu}_{D}\\) or one superpixel node in \\(\large{\nu}_{S}\\) belonging to a certain target; \\(\psi(v,w)\\) is pairwise edges among superpixels and detections, including spacial and temporal information among superpixels and information among superpixels and detections in the same frame; \\(\psi^{\lambda}\\) is trajectory cost, containing several constrains of height, shape, dynamics, persistence, image likelihood and parsimony.

### ADVANTAGES ###

1. Taking pixel (superpixel) level information in addition to detection results into consideration could handle partial occlusions, which would lead to higher recall.
2. Segments could provide considerable information even no reliable detection result exists.
3. Modeling multi-targets tracking problem to graph model could take advantage of existing optimization algorithms.

### DISADVANTAGES ###

1. Solving CRF problem is slow, needing 12 seconds per frame.
2. Can not handle ID switch in two adjacent temporal slidewindows.

### OTHER ###
1. Tracking-by-detection has proven to be the most successful strategy to address multi-target tracking problem.
2. Noise and imprecise measurements, long-term occlusions, complicated dynamics and target interactions all contributes to the problem's complexity.