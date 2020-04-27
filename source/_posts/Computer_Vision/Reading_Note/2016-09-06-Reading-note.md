---
date: 2016-09-06 00:00:00
title: "What makes ImageNet good for transfer learning?"
category: ["Computer Vision"]
tag: ["Reading Note", "CNN"]
---

**TITLE**: What makes ImageNet good for transfer learning?

**AUTHOR**: Minyoung Huh, Pulkit Agrawal, Alexei A. Efros

**ASSOCIATION**: Berkeley Artificial Intelligence Research (BAIR) Laboratory, UC Berkeley

**FROM**: [http://arxiv.org/abs/1608.08614](http://arxiv.org/abs/1608.08614)

### CONTRIBUTIONS ###

Several questions about how the dataset affects the training of CNN is discussed, including

* Is more pre-training data always better? How does feature quality depend on the number of training examples per class? 
* Does adding more object classes improve performance? 
* For the same data budget, how should the data be split into classes?
* Is fine-grained recognition necessary for learning good features?
*  Given the same number of training classes, is it better to have coarse classes or fine-grained classes? 
*  Which is better: more classes or more examples per class?

### Summary ###

>The following is a summary of the main findings:
>
>1. **How many pre-training ImageNet examples are sufficient for transfer learning?** Pre-training with only half the ImageNet data (500 images per class instead of 1000)results in only a small drop in transfer learning performance (1.5 mAP drop on PASCAL-DET). This drop is much smaller than the drop on the ImageNet classification task itself.
>2. **How many pre-training ImageNet classes are sufficient for transfer learning?** Pre-training with an order of magnitude fewer classes (127 classes instead of 1000) results in only a small drop in transfer learning performance (drop of 2.8 mAP on PASCAL-DET). Quite interestingly, we also found that for some transfer tasks, pre-training with fewer number of classes leads to better performance.
>3. **How important is fine-grained recognition for learning good features for transfer learning?** The above experiment also suggests that transferable features are learnt even when a CNN is pre-trained with a set of classes that do not require fine-grained discrimination.
>4. **Given the same budget of pre-training images, should we have more classes or more images per class?** Training with fewer classes but more images per class performs slightly better than training with more classes but fewer images per class.
>5. **Is more data always helpful?** We found that training using 771 ImageNet classes that excludes all PASCAL VOC classes, achieves nearly the same performance on PASCALDET as training on complete ImageNet. Further experiments confirm that blindly adding more training data does not always lead to better performance and can sometimes hurt performance.