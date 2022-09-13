---
layout: post
title: Batch Normalization에서의 Backpropagation
description: 'Batch Normalization에서의 Backpropagation'
date: '2022-09-13 15:00:00 +0900'
categories:
  - Study
  - CS231n
tags:
  - CS231n
  - ML/AI
toc: true
cover: /images/Stanford_Cover.png
thumbnail: /images/Stanford_Thumbnail.png
noindex: false
sitemap: true
---

CS231n assignment2의 `BatchNormalization.ipynb` 의 풀이 과정에서 Batch Normalization Backward flow 가 요구되는데, 이를 해결한 두 가지 방법을 간단히 소개하고자 한다. 각각의 아이디어가 `batchnorm_backward` 와 `batchnorm_backward_alt` 에 사용되었다. 


<!-- more -->

## 풀이 1) computational graph을 직접 그려 한 단계씩 계산

<img src='../../../../images/post/Batch_Normalization_Backprop_Img1.jpg' alt='Image1: Batchnorm Backprop 풀이 1' style="display: block; margin: 0 auto"> </img>


## 풀이 2) 편미분과 연쇄법칙을 직접 활용한 수학적 계산

<img src='../../../../images/post/Batch_Normalization_Backprop_Img2.jpg' alt='Image2: Batchnorm Backprop 풀이 2' style="display: block; margin: 0 auto"> </img>