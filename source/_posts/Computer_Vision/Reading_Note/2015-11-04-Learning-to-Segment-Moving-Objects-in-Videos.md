---
date: 2015-11-04 00:00:00
title: "READING NOTE: Learning to Segment Moving Objects in Videos"
category: ["Computer Vision"]
tag: ["Reading Note", "Segmentation", "Video"]
---

**TITLE**: Learning to Segment Moving Objects in Videos

**AUTHOR**: Fragkiadaki, Katerina and Arbelaez, Pablo and Felsen, Panna and Malik, Jitendra

**FROM**: CVPR2015

### CONTRIBUTIONS ###

1. Moving object proposals from multiple segmentations on optical flow boundaries
2. A moving objectness detector for ranking per frame segments and tube proposals
3. A method of extending per frame segments into spatial-temporal tubes

### METHOD ###

1. Extract motion boundaries by optical flow
2. Generate segment proposals according to motion boundaries, called MOPs (Moving Object Proposal)
3. Rank the MOPs using a CNN based regressor
4. Combine per frame MOPs to space-time tubes based on pixelwise trajectory clusters

### ADVANTAGES ###

1. Using optical flow could reduce the noises caused by inner texture of one object. Optical flow is more suitable for detecting rigid objects.
2. Using trajectory tracking could deal with objects that are temporary static.
3. Segments are effective to tackle frequent occlusions/dis-occlustions.

### DISADVANTAGES ###

1. Too slow. Every stage would take seconds to process, which is not suitable for practical applications.
2. Use several independent method to detect objects. Less computations are shared.
3. The power of CNN has not been fully applied.

### OTHER ###
1. RCNN has excellent performance on object detection in static images
2. For slidewindow methods, too many patches need to be evaluated.
3. MRF methods neglect nearby pixels' relation and could not separate adjacent instances.
4. Methods of object detection in video could be categorized into two types i) top-down tracking and ii) bottom-up segmentation.