---
date: 2018-09-11 00:00:00
title: "Training MTCNN"
category: "Computer Vision"
tag: ["Face Detection", "Face Alignment"]
---

[//]: <> (# 训练步骤 #)
[//]: <> (1. 测试负样本，)

# MTCNN 训练记录

最近尝试使用 Caffe 复现 MTCNN，感觉坑很大，记录一下训练过程，目前还没有好的结果。网上也有很多童鞋在尝试训练 MTCNN，普遍反映使用 TensorFlow 可以得到比较好的结果，但是使用 Caffe 不是很乐观。

## 已经遇到的问题现象

- 2018.09.11：目前只训练了 12net，召回率偏低。

  以[blankWorld/MTCNN-Accelerate-Onet](https://github.com/blankWorld/MTCNN-Accelerate-Onet)为 baseline，blankWorld 在 FDDB 上的测试性能如下图

  ![FDDB Result](https://raw.githubusercontent.com/blankWorld/MTCNN-Accelerate-Onet/master/img/mtcnn-fddb.jpg "FDDB Result")

  这个效果很不错，但是我自己生成样本后训练 12net，召回率有明显下降。性能对比如下图

  ![FDDB 12net Compare](https://raw.githubusercontent.com/joshua19881228/joshua19881228.github.io/master/img/TrainMTCNN/12net_fddb.png "FDDB 12net Compare")

  暂且不管 12net 的测试结果为什么会这么差，两个模型的性能差距是可以反映的。

## 训练记录

- 2018.09.11

  **训练数据生成**

  参考[AITTSMD/MTCNN-Tensorflow](https://github.com/AITTSMD/MTCNN-Tensorflow)提供的 prepare_data 进行数据生成。数据集情况如下表

  |                    |   Positive    |   Negative    |     Part      |   Landmark    |
  | :----------------: | :-----------: | :-----------: | :-----------: | :-----------: |
  |  **Training Set**  | 156728/189530 | 470184/975229 | 156728/547211 | 313456/357604 |
  | **Validation Set** |     10000     |     10000     |     10000     |     10000     |

  其中 Pos:Neg:Part:Landmark = 1:3:1:2，样本比例参考原作的比例。Pos、Neg、Part 来自于[WiderFace](http://mmlab.ie.cuhk.edu.hk/projects/WIDERFace/index.html)，Landmark 来自于[CelebA](http://mmlab.ie.cuhk.edu.hk/projects/CelebA.html)。其中正样本进行了人工的数据筛选，筛选的原因是根据 WiderFace 生成的正样本，有很多都是质量很差的图像，包含人脸大面积遮挡或十分模糊的情况。之前召回率很差的性能来自没有经过筛选的训练集，因为使用了 OHEM，只有 loss 值在前 70%的样本才参与梯度计算，感觉如果质量差的样本占比较大，网络学习到的特征是错误的，那些质量好的图像可能得不到充分的学习。

  **训练参数设置**

  初始训练参数如下

  ```protobuf
  type:"Adam"
  momentum: 0.9
  momentum2:0.999
  delta:1e-8
  base_lr: 0.01
  weight_decay: 0.0005
  batch_size: 256
  ```

  [//]: <> (训练路径在 62 服务器的/data2/zxli/CODE/caffe_multilabel/examples/mtcnn_12net/下，模型 models_20180907，数据 data_20180907，记录 train_20180911。图像数据存储在/data2/zxli/GIT/mtcnn-caffe/prepare_data/12_20180905/)

  第一轮训练在 75000 次迭代(17.5 个 epoch)时停止，测试记录如下

  ```vim
  I0911 10:16:25.019253 21722 solver.cpp:347] Iteration 75000, Testing net (#0)
  I0911 10:16:28.057858 21727 data_layer.cpp:89] Restarting data prefetching from start.
  I0911 10:16:28.072748 21722 solver.cpp:414]     Test net output #0: cls_Acc = 0.4638
  I0911 10:16:28.072789 21722 solver.cpp:414]     Test net output #1: cls_loss = 0.096654 (* 1 = 0.096654 loss)
  I0911 10:16:28.072796 21722 solver.cpp:414]     Test net output #2: pts_loss = 0.008529 (* 0.5 = 0.0042645 loss)
  I0911 10:16:28.072801 21722 solver.cpp:414]     Test net output #3: roi_loss = 0.0221648 (* 0.5 = 0.0110824 loss)
  ```

  **注意：分类测试结果是 0.4638 是因为测试集没有打乱，1-10000 为 pos 样本，10001-20000 为 neg 样本，20001-30000 为 part 样本，30001-40000 为 landmark 样本。因此，实际分类正确率应该是 0.9276**

  降低学习率至 0.001，训练 135000 次迭代(31.5 个 epoch)时停止，测试记录如下

  ```vim
  I0911 13:14:36.482010 23543 solver.cpp:347] Iteration 135000, Testing net (#0)
  I0911 13:14:39.629933 23660 data_layer.cpp:89] Restarting data prefetching from start.
  I0911 13:14:39.645612 23543 solver.cpp:414]     Test net output #0: cls_Acc = 0.4714
  I0911 13:14:39.645649 23543 solver.cpp:414]     Test net output #1: cls_loss = 0.0765401 (* 1 = 0.0765401 loss)
  I0911 13:14:39.645656 23543 solver.cpp:414]     Test net output #2: pts_loss = 0.00756469 (* 0.5 = 0.00378234 loss)
  I0911 13:14:39.645661 23543 solver.cpp:414]     Test net output #3: roi_loss = 0.0201988 (* 0.5 = 0.0100994 loss)
  ```

  实际分类正确率是 0.9428。训练 260000 次迭代后停止，测试记录如下

  ```vim
  I0911 16:58:47.514267 28442 solver.cpp:347] Iteration 260000, Testing net (#0)
  I0911 16:58:50.624385 28448 data_layer.cpp:89] Restarting data prefetching from start.
  I0911 16:58:50.639556 28442 solver.cpp:414]     Test net output #0: cls_Acc = 0.471876
  I0911 16:58:50.639595 28442 solver.cpp:414]     Test net output #1: cls_loss = 0.0750447 (* 1 = 0.0750447 loss)
  I0911 16:58:50.639602 28442 solver.cpp:414]     Test net output #2: pts_loss = 0.0074394 (* 0.5 = 0.0037197 loss)
  I0911 16:58:50.639608 28442 solver.cpp:414]     Test net output #3: roi_loss = 0.0199694 (* 0.5 = 0.00998469 loss)
  ```

  实际分类正确率是 0.943752。

  **问题：** 训练结果看似还可以，但是召回率很低，在阈值设置为 0.3 的情况下，召回率也才将将达到 90%。阈值要设置到 0.05，才能达到 97%-98%的召回率，ROC 曲线如下图。严格来说这个测试并不严谨，应该用检测器直接在图像中进行检测，但是为了方便，我直接用 val 集上的性能画出了 ROC 曲线，其中的 FDDB 曲线是将的人脸区域截取出来进行测试得到的。

  ![12net 1st ROC](https://raw.githubusercontent.com/joshua19881228/joshua19881228.github.io/master/img/TrainMTCNN/12net_roc_1st.png "12net 1st ROC")

- 2018.09.14

  使用上述 12net 在 WiderFace 上提取正负样本，提取结果如下：

  | Thresholed | Positive | Negative |  Part  |
  | :--------: | :------: | :------: | :----: |
  |    0.05    |  85210   | 36745286 | 632861 |
  |    0.5     |  66224   | 6299420  | 354350 |

- 2018.09.17

  准备 24net 的训练样本。由于生成 12net 检测到的正样本数目有限，训练 24net 的 pos 样本包含两部分，一部分是训练 12net 的正样本，一部分是经过筛选的 12net 检测到的正样本；neg 样本和 part 样本全部来自 12net 的难例；landmark 与 12net 共用样本。经过采样后达到样本比例 1:3:1:2，样本数目如下表：

  |                    | Positive | Negative |  Part  | Landmark |
  | :----------------: | :------: | :------: | :----: | :------: |
  |  **Training Set**  |  225172  |  675516  | 225172 |  313456  |
  | **Validation Set** |  10000   |  10000   | 10000  |  10000   |

  [//]: <> (训练路径在 62 服务器的/data2/zxli/CODE/caffe_multilabel/examples/mtcnn_24net/下，模型 models_20180917，数据 data_20180916，记录 train_20180917。图像数据存储在/data2/zxli/GIT/mtcnn-caffe/prepare_data/24_20180914/)

  训练过程与 12net 类似，学习率从 0.01 下降到 0.0001，最终的训练结果如下

  ```vim
  I0917 15:19:00.631140 36330 solver.cpp:347] Iteration 70000, Testing net (#0)
  I0917 15:19:03.305665 36335 data_layer.cpp:89] Restarting data prefetching from start.
  I0917 15:19:03.317827 36330 solver.cpp:414]     Test net output #0: cls_Acc = 0.481501
  I0917 15:19:03.317865 36330 solver.cpp:414]     Test net output #1: cls_loss = 0.0479137 (* 1 = 0.0479137 loss)
  I0917 15:19:03.317874 36330 solver.cpp:414]     Test net output #2: pts_loss = 0.00631254 (* 0.5 = 0.00315627 loss)
  I0917 15:19:03.317879 36330 solver.cpp:414]     Test net output #3: roi_loss = 0.0179083 (* 0.5 = 0.00895414 loss)
  ```

  实际分类正确率是 0.963。ROC 曲线如下图，同样使用 val 集上的性能画出曲线。

  ![24net 1st ROC](https://raw.githubusercontent.com/joshua19881228/joshua19881228.github.io/master/img/TrainMTCNN/24net_roc_1st.png "24net 1st ROC")

- 2018.09.18

  使用 24net 在 WiderFace 上提取正负样本，提取结果如下：

  | Thresholed | Positive | Negative |  Part  |
  | :--------: | :------: | :------: | :----: |
  |  0.5, 0.5  |  86396   |  83212   | 225285 |

  利用以上数据生成 48net 的训练样本，由于 24net 生成的样本数量有限，结合前两次训练所用的数据，生成训练集：

  |                    | Positive | Negative |  Part  | Landmark |
  | :----------------: | :------: | :------: | :----: | :------: |
  |  **Training Set**  |  283616  |  850848  | 283616 |  567232  |
  | **Validation Set** |  10000   |  10000   | 10000  |  10000   |

- 2018.09.19

  在训练 48net 的过程中，首先尝试了 Adam 算法进行优化，后来发现训练十分不稳定。转而使用 SGD 进行优化，效果好转。训练初始参数如下：

  ```protobuf
  type:"SGD"
  base_lr: 0.01
  momentum: 0.9
  weight_decay: 0.0005
  ```

  48net 的训练结果比较一般，性能如下:

  ```vim
  I0919 18:02:22.318362  3822 solver.cpp:347] Iteration 165000, Testing net (#0)
  I0919 18:02:25.877437  3827 data_layer.cpp:89] Restarting data prefetching from start.
  I0919 18:02:25.894898  3822 solver.cpp:414]     Test net output #0: cls_Acc = 0.4662
  I0919 18:02:25.894937  3822 solver.cpp:414]     Test net output #1: cls_loss = 0.0917524 (* 1 = 0.0917524 loss)
  I0919 18:02:25.894943  3822 solver.cpp:414]     Test net output #2: pts_loss = 0.00566356 (* 1 = 0.00566356 loss)
  I0919 18:02:25.894948  3822 solver.cpp:414]     Test net output #3: roi_loss = 0.0177907 (* 0.5 = 0.00889534 loss)
  ```

  实际的分类精度为 0.9324。整体来看基本实现了文章中[参考文献[19]](http://users.eecs.northwestern.edu/~xsh835/assets/cvpr2015_cascnn.pdf)在验证集上的性能，性能对比如下表

  |  CNN  | 12-net | 24-net | 48-net |
  | :---: | :----: | :----: | :----: |
  | [19]  | 94.4%  | 95.1%  | 93.2%  |
  | MTCNN | 94.6%  | 95.4%  | 95.4%  |
  | Ours  | 94.3%  | 96.3%  | 93.2%  |

- 2018.09.20

  整个系统连通后进行测试，发现人脸框抖动比较厉害，这应该是训练过程和样本带来的问题。

  比较奇怪的问题是在 Caffe 上进行 CPU 运算时，速度极慢，尤其 12net 运行速度慢 30 倍左右。通过观察参数分布发现，有大量 kernel 都是全零分布，初步感觉是因为 Adam 和 ignore label 相互作用的结果，即 ignore label 的样本会产生 0 值 loss，这些 loss 会影响 Adam 的优化过程，具体原因还需进一步理论推导。目前的解决方案是将含有大量 0 值 kernel 的层随机初始化，使用 SGD 进行训练。至于抖动的问题，需要进一步分析。重训后的模型性能如下表：

  |          | 12-net | 24-net | 48-net |
  | :------: | :----: | :----: | :----: |
  | Accuracy | 94.59% | 96.52% | 93.94% |

- 2018.09.26

  目前来看回归器的训练是完全失败。失败的原因可能有以下几点：

  1. 对欧拉损失层做了代码修改，用来支持 ignore label，可能代码出现问题
  2. 数据本身存在问题，需要验证数据的正确性

  先训练一个不带 ignore label 的回归器，看 loss 是否发生变化。

- 2018.09.27

  做了一个实验，用 20 张图像生成一个数据集，经过训练，网络是可以完全过拟合这个数据集的，说明数据和代码是没有问题的，但是加大数据量后，bounding box 的回归依旧不好。通过跟大神讨论，发现自己犯了一个很弱智的低级错误，**回归问题是不可以做镜像的，回归问题是不可以做镜像的，回归问题是不可以做镜像的**，重要的事情说三遍，如果图像做镜像操作，那么标注也需要进行镜像操作。

  重训后的模型性能如下表：

  |          | 12-net | 24-net | 48-net |
  | :------: | :----: | :----: | :----: |
  | Accuracy | 94.35% | 97.45% | 94.67% |

- 2018.10.01

  实验还是不够顺利，有些受挫感。目前来看 12net 和 24net 的训练是相对顺利的，至少能够去除大量虚检并给出相对准确的回归框。48net 的训练比较失败，存在虚检、定位不准以及关键点定位不准的问题。

  在训练 12net 和 24net 时，因为网络较小，而且这两个网络都不负责输出关键点，所以训练时只训练了分类和框回归问题。训练 48net 时三个 loss 都很重要，感觉按照原文的 loss weight 进行配置会造成回归问题学习不充分的问题，需要提高回归问题的 loss weight。

  接下来的工作包括：

  1. 之前的训练为了快速验证思路，并没有严格按照后一阶段数据是前一阶段数据中的难例的原则，接下来需要重新走这个流程。
  2. 探索 48net 的训练过程。具体来说有几个点需要尝试，首先，是刚刚提到的难例挖掘；其次，调整各个 loss 的权重；最后，除了阶段间的难例挖掘，也应注意阶段内的难例挖掘。
  3. 以 24net 在 landmark 数据集上进行检测，将其输出作为 48net 进行 landmark 回归的输入。

- 2018.10.13

  最近一次训练的记录如下：

  **12net**：

  12net 的样本由随机采样得来。

  |                    | Positive | Negative |  Part  |
  | :----------------: | :------: | :------: | :----: |
  |  **Training Set**  |  156728  |  470184  | 156728 |
  | **Validation Set** |  10000   |  10000   | 10000  |

  验证集上的性能为：

  ```vim
  Test net output #0: cls_Acc = 0.9435
  Test net output #1: cls_loss = 0.0747717 (* 1 = 0.0747717 loss)
  Test net output #2: roi_loss = 0.0168385 (* 0.5 = 0.00841924 loss)
  ```

  **24net**

  24net 的样本全部来自 12net 的检测结果。

  |                    | Positive | Negative |  Part  |
  | :----------------: | :------: | :------: | :----: |
  |  **Training Set**  |  60149   |  180447  | 120298 |
  | **Validation Set** |   1500   |   1500   |  1500  |

  验证集上的性能为：

  ```vim
  Test net output #0: cls_Acc = 0.977588
  Test net output #1: cls_loss = 0.0648633 (* 1 = 0.0648633 loss)
  Test net output #2: roi_loss = 0.0192365 (* 5 = 0.0961826 loss)
  ```

  **48net**

  48net 的正样本和 part 样本来自于 24net 在 widerface 上的检测结果，负样本来自于 24net 在 widerface 和 celeba 上的检测结果，landmark 样本来自于 24net 在 celeba 上的检测结果。

  |                    | Positive | Negative |  Part  | landmark |
  | :----------------: | :------: | :------: | :----: | :------: |
  |  **Training Set**  |  242862  |  728586  | 242862 |  485724  |
  | **Validation Set** |   5000   |   5000   |  5000  |   5000   |

  在验证集上的性能为：

  ```vim
  Test net output #0: cls_Acc = 0.978155
  Test net output #1: cls_loss = 0.0694968 (* 1 = 0.0694968 loss)
  Test net output #2: pts_loss = 0.00119616 (* 5 = 0.00598078 loss)
  Test net output #3: roi_loss = 0.0111277 (* 1 = 0.0111277 loss)
  ```

  使用 0.5,0.5,0.5 作为阈值，在 FDDB 上测得的 discROC 曲线如下图

  ![ROC 20181013](https://raw.githubusercontent.com/joshua19881228/joshua19881228.github.io/master/img/TrainMTCNN/discROC_20181013.png "ROC 20181013")
