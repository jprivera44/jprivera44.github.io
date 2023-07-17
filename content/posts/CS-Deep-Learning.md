---
title: "Exploring Transformers applied to NeRF"
date: 2023-05-04T23:13:01-07:00
draft: false
---


#

<img src="/posts/images/paper_img.png" alt="My Image" style="width: 700px;">
<small> Above is the first page from the paper, link is below for full paper.</small>

# 

This page briefly describes my invovlement within the end of semester deep learning project for Georgia Tech's CS 7643 Deep learning course. I reimplemented the decoder portion of a ray transformer, that focused on reconstructing the output fromt he encoder. The link to the project, and the Original paper (Is attention all the NeRF needs Varma et al.) are below.

I was specifically in charge of the lgith ray transformer. This component is a significant advancement over classical rendering techniques. Instead of relying on a rigid, pre-determined equation, we utilized a learned representation, bringing adaptability and flexibility to the rendering process.

A significant part of my work involved the computation of feature representations for each point sampled on a ray. I worked on integrating position encoding of spatial location and view direction. These features were then processed by the Ray Transformer to derive the color. Additionally I was in charge of training the model with the new implementations of code from myself, and my partner Mohammed. I ran the full training loop on an Nvidia A100, for about 20 hours to get reasonable results.

What I found particularly fascinating was how the Ray Transformer could automatically modulate attention distribution. This ability to control the sharpness of the reconstructed surface and capture intricate lighting effects was a testament to the power of learning-based methods. By harnessing the expressive capabilities of the image encoder, we could simulate complex light transport phenomena such as refraction and reflection.




Here's a [link](https://github.com/jprivera44/GaTech-cs7643/blob/main/Project_Report_CS_7643_NeRF_%26_GNT.pdf) to the class project.


Here's a [link](https://arxiv.org/abs/2207.13298) Original Nerf tranformer paper.
