---
date: 2016-08-09 00:00:00
title: "Reading Note: Do semantic parts emerge in Convolutional Neural Networks"
category: ["Computer Vision"]
tag: ["Reading Note"]
---

**TITLE**: Do semantic parts emerge in Convolutional Neural Networks?

**AUTHER**: Abel Gonzalez-Garica, David Modolo, Vittorio Ferrari

**ASSOCIATION**: CLAVIN, University of Edingburgh, UK

**FROM**: [arXiv:1607.03738](http://arxiv.org/abs/1607.03738)

### CONTRIBUTIONS ###

1. An extensive quantitative analysis of the association between responses of CNN filters and sematic parts 

### METHOD ###

1. CNNs are trained for object detection task or object classification.
2. Filters that give significant responses to certain semantic parts are selected.
3. Filters are comibned to construct a part detector if necessary.
4. A regressor is trained for part bounding-boxes.
5. Discriminative filters are selected in object classification task.

### Observation ###

There are several interesting observatoins from the authers.

**Differences between layers.** Overall, the higher the network layer, the higher the performance. It means that in higher part of the network abstract semantic contents are represented.

**Differences between part classes.** Performance varies greatly across part classes. It seems that very discriminative semantic parts are well detected.

**Filter combinations.** Performing part detection using a combination of filters always performs better than single best filter. It means taht a semantic part may be represented jointly by several filters.

**Filter sharing across part classes.** Filters are shared across different part classes. It is clear that some filters are representative for a generic part and work well on all object classes containing it.

**The number of emerged semantic parts.** Only a modest number of filters responses to semantic parts. The auther concludes that the network does contain filters combinations that can cover some part classes well, but they do not fire exclusively on the part, making them weak part detectors. Moreover, the part classes covered by the semantic filters tend to either cover a large image area, or be very discriminative for their object class.

**Discriminative filters in object classification.** The filters are measured by how much they contribute to the classification score. On average, 9/256 filters are discriminative for a particular class. The total number of dicriminative filte overall 16 object classes amounts to 104. It shows that the discriminative filters are largely distributed across different object classes, with very little sharing.

**Discriminative and semantic filters.** 5.5 out of the 9 discriminative filters for an object class are semantic filters.It means that only a portion of the filters learned by CNN are semantic, and many are just responding to dicriminative patches.