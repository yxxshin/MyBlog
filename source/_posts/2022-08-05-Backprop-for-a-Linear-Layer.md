---
layout: post
title: Backpropagation for a Linear Layer
description: 'Matrix 들에서의 Backpropagation (Chain Rule) 계산'
date: '2022-08-05 15:00:00 +0900'
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

CS231n assignment1 의 `two_layer_net.ipynb` 해결에 필수적이었던, **Matrix들에서의 Backpropagation (Chain Rule)** 에 대하여 설명한다. 원문 파일은 아래와 같다.  
[Backpropagation for a Linear Layer](http://cs231n.stanford.edu/handouts/linear-backprop.pdf)

<!-- more -->

## 목표
$Y$, $X$, $W$ 가 모두 행렬이고 $L$ 이 스칼라량이라 하자.  
**${\partial L \over \partial Y }$ 를 알고 있을 때, ${\partial L \over \partial X}$ 와 ${\partial L \over \partial W}$ 를 계산**하는 것이 목표이다. 이것이 기본적인 Chain Rule을 이용한 Backpropagation이다. 

## 조건
직접 Chain Rule을 이용하면, ${\partial Y \over \partial X}$ 나 ${\partial Y \over \partial W}$ 와 같은 항들이 등장한다. 이 항들은 Jacobian Matrix 인데, 너무나 크기 때문에 이 **Jacobian 항들을 쓰고 싶지 않다**.

## 방법
위의 원문 파일에서는 **one element at a time** 방식을 이용한다. 행렬을 원소 단위로 나누어 생각하고 계산해 본다는 것인데, 작은 사이즈의 경우를 예시로 직접 계산한 뒤 이를 일반화하였다.

## 결론
$$ {\partial L \over \partial X} = {\partial L \over \partial Y} \cdot W^T $$
$$ {\partial L \over \partial W} = X^T \cdot {\partial L \over \partial Y} $$

## 참고
- ${\partial (\text{scalar}) \over \partial(\text{matrix})} $ 와 ${\partial (\text{matrix}) \over \partial(\text{scalar})} $ 모두 결과값의 shape는 matrix의 shape와 같다!
- 원문에서 두 matrix의 dot product와 곱셈(multiplication)을 구분하는 듯 하였는데, 개인적으로 이 용법은 잘못되었다고 생각한다. Dot product는 두 matrix 사이에서 정의되지 않으며 조금 더 포괄적으로 생각하더라도 행렬들에 해당되는 dot product는 행렬의 곱셈이다. (그래서 필자와 같이 헷갈리는 사람을 만들어낸다 ㅜㅜ) 의미상 행렬의 dot product를 스칼라량이 output으로 나오는 *inner product*로 생각하면 깔끔하게 맞아 떨어진다. ($ < A,B > = \sum_{1 \leq i, j \leq n} a_{ij}b_{ij}$)