---
layout: post
title: SVM, Softmax No loop 코드 아이디어
description: 'CS231n assignment1 '
date: '2022-07-19 15:00:00 +0900'
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

# Assignment1 : svm.ipynb & softmax.ipynb
본 게시물에서는 CS231n의 assignment1 (Spring 2020) 중, [svm.ipynb](https://github.com/yxxshin/CS231N_assignments/blob/main/assignment1/svm.ipynb) 과제와 [softmax.ipynb](https://github.com/yxxshin/CS231N_assignments/blob/main/assignment1/softmax.ipynb) 과제의 주요 아이디어에 대하여 설명한다. 하이퍼링크를 통해 필자가 작성한 assignment solution을 확인할 수 있다.

<!-- more -->

## 목표
`svm.ipynb`, `softmax.ipynb` 의 과제에서는 최종적으로 각각 SVM, Softmax classifier에 대한 **fully-vectorized loss function**을 구현하고, 이 loss function의 **fully-vectorized analytic gradient**를 구현하여 SGD를 이용하여 optimization을 진행하는 것이다. 본 게시물에서는 위의 'fully-vectorized' 한 부분들의 아이디어를 설명하고자 한다. fully-vectorized는 결국 *loop이 없다*는 것을 의미하는데, 이에는 python skill들과 아이디어가 필요하기 때문이다. 

## svm.ipynb
시작하기에 앞서, Multiclass SVM 에서의 loss function은 다음과 같다.

$$ L_i = \sum_{j \neq y_i} 
  \begin{cases}
    0 & \text{if} \enspace s_{y_i} \geq s_j + 1   \\
    s_j - s_{y_i} + 1 & \text{otherwise}  
  \end{cases} 
  
  \\[2em]
  
  = \sum_{j \neq y_i} \text{max}(0, s_j - s_{y_i}+1)
$$

편의를 위해 regularization term은 생략하도록 하자. (계산 마지막에 더해주기만 하면 된다) 다음과 같이 Indicator function을 정의하자.

$$ I(x) = 
  \begin{cases}
    1 & \text{if} \enspace x > 0 \\
    0 & \text{else}
  \end{cases}
$$

위의 loss function을 직접 미분해보면, 다음과 같은 편미분 값을 얻는다.
$$ j \neq y_i : \qquad {\partial L_i \over \partial w_j} = I(x_i \cdot w_j - x_i \cdot w_{y_i} + 1) \cdot x_i^T $$
$$ j = y_i : \qquad {\partial L_i \over \partial w_{y_i}} = (\enspace \sum_{j \neq y_i} I(x_i \cdot w_j - x_i \cdot w_{y_i} + 1) \enspace ) \cdot (-x_i^T)  $$

여기서 $x_i$는 $X$의 $i$번째 row vector, $w_j$는 $W$의 $j$번째 column vector 이다. (notation 주의!)  
이 식들을 이용하여 $ L = {1 \over N} \sum_{i = 1}^{N} L_i $ 를 통해 total loss L 을 구한다. 