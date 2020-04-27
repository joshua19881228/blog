---
date: 2018-01-08 00:00:00
title: "Reading Note: Panoptic Segmentation"
category: ["Computer Vision"]
tag: ["Reading Note", "Segmentation"]
---

**TITLE**: Panoptic Segmentation

**AUTHOR**: Alexander Kirillov, Kaiming He, Ross Girshick, Carsten Rother, Piotr Dollar

**ASSOCIATION**: FAIR, Heidelberg University

**FROM**: [arXiv:1801.00868](https://arxiv.org/abs/1801.00868)

## CONTRIBUTION ##

1. A novel ‘Panoptic Segmentation’ (PS) task is proposed and studied.
2. A panoptic quality (PQ) measure is introduced to measure performance on the task.
3. A basic algorithmic approach to combine instance and semantic segmentation outputs into panoptic outputs is proposed.

## PROBLEM DEFINATION ##

*Panoptic* refers to a unified, global view of segmentation. Each pixel of an image must be assigned a semantic label and an instance id. Pixels with the same label and id belong to the same object; for stuff labels the instance id is ignored.

### Panoptic Segmentation ###

Given a predetermined set of $L$ semantic categories encoded by $\mathcal{L} := \{1,...,L\}$, the task requires a panoptic segmentation algorithm to map each pixel $i$ of an image to a pair $(l_{i}, z_{i}) \in \mathcal{L} \times N$, where $l_{i}$ represents the semantic class of pixel $i$ and $z_{i}$ represents its instance id.

The semantic label set consist of subsets $\mathcal{L}^{St}$ and $\mathcal{L}^{Th}$, such that $\mathcal{L} = \mathcal{L}^{St} \cup \mathcal{L}^{Th}$ and $\mathcal{L}^{St} \cap \mathcal{L}^{Th} = \phi$. These subsets correspond to *stuff* labels and *thing* labels, respectively.

### Panoptic Quality (PQ) ###

For each class, the unique matching splits the predicted and ground truth segments into three sets: true positives (TP), false positives (FP), and false negatives (FN), representing matched pairs of segments, unmatched predicted segments, and unmatched ground truth segments, respectively. Given these three sets, PQ is defined as:

$$PQ=\frac{\sum_{(p,g) \in TP} IoU(p,g)}{|TP|+\frac{1}{2}|FP|+\frac{1}{2}|FN|}$$

A predicted segment and a ground truth segment can match only if their intersection over union (IoU) is strictly greater than 0.5.

PQ can be seen as the multiplication of a *Segmentation Quality* (SQ) term and a *Detection Quality* (DQ) term:

$$PQ=\frac{\sum_{(p,g) \in TP} IoU(p,g)}{|TP|} \times \frac{|TP|}{|TP|+\frac{1}{2}|FP|+\frac{1}{2}|FN|}$$

where the first term can be seen as SQ and the second term can be seen as DQ.

### Human vs. Machine ###

![Human vs. Machine](https://raw.githubusercontent.com/joshua19881228/my_blogs/master/Computer_Vision/Reading_Note/figures/Reading_Note_20180109_human-vs-machine.png "Human vs. Machine")