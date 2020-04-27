---
date: 2017-10-31 00:00:00
title: "Reading Note: Be Your Own Prada: Fashion Synthesis with Structural Coherence"
category: ["Computer Vision"]
tag: ["Reading Note", "CNN", "GAN", "Fashion"]
---

**TITLE**: Be Your Own Prada: Fashion Synthesis with Structural Coherence

**AUTHOR**: Shizhan Zhu, Sanja Fidler, Raquel Urtasun, Dahua Lin, Chen Change Loy

**ASSOCIATION**: The Chinese University of Hong Kong, University of Toronto, Vector Institute, Uber Advanced Technologies Group

**FROM**: [ICCV2017](http://personal.ie.cuhk.edu.hk/~ccloy/files/iccv_2017_fashiongan.pdf)

## CONTRIBUTION ##

A method that can generate new outfits onto existing photos is developped so that it can 

1. retain the body shape and pose of the wearer,
2. roduce regions and the associated textures that conform to the language description, 
3. Enforce coherent visibility of body parts.

## METHOD ##

Given an input photograph of a person and a sentence description of a new desired outfit, the model first generates a segmentation map $\tilde{S}$ using the generator from the first GAN. Then the new image is rendered with another GAN, with the guidance from the segmentation map generated in the previous step. At test time, the final rendered image is obtained with a forward pass through the two GAN networks. The workflow of this work is shown in the following figure.

![Framework](https://raw.githubusercontent.com/joshua19881228/my_blogs/master/Computer_Vision/Reading_Note/figures/Reading_Note_20171031_DeepFashion.png "Framework")

The first generator $G_{shape}$ aims to generate the desired semantic segmentation map $$\tilde{S}$$ by conditioning on the spatial constraint $$\downarrow m(S_0)$$, the design coding $$\textbf{d}$$, and the Gaussian noise $$\textbf{z}_{S}$$. $$S_{0}$$ is the original pixel-wise one-hot segmentation map of the input image with height of $$m$$, width of $n$ and channel of $L$, which represents the number of labels. $\downarrow m(S_0)$ downsamples and merges $S_{0}$ so that it is agnostic of the clothing worn in the original image, and only captures information about the user's body. Thus $G_{shape}$ can generate a segmentation map $\tilde{S}$ with sleeves from a segmentation map $S_{0}$ without sleeves.

The second generator $G_{image}$ renders the final image $\tilde{I}$ based on the generated segmentation map $\tilde{S}$, design coding $\textbf{d}$, and the Gaussian noise $\textbf{z}_I$.