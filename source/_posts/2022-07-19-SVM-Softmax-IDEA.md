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
본 게시물에서는 CS231n의 assignment1 (Spring 2020) 중, [svm.ipynb](https://github.com/yxxshin/CS231N_assignments/blob/main/assignment1/svm.ipynb) 과제와 [softmax.ipynb](https://github.com/yxxshin/CS231N_assignments/blob/main/assignment1/softmax.ipynb) 과제의 주요 아이디어에 대하여 설명한다. 하이퍼링크를 통해 필자가 작성한 전체 assignment solution을 확인할 수 있으며, loss function과 gradient를 구하는 코드는 [linear_svm.py](https://github.com/yxxshin/CS231N_assignments/blob/main/assignment1/cs231n/classifiers/linear_svm.py) 와 [softmax.py](https://github.com/yxxshin/CS231N_assignments/blob/main/assignment1/cs231n/classifiers/softmax.py) 에서 확인 가능하다.

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

편의를 위해 regularization term은 생략하도록 하자. (계산 마지막에 더해주기만 하면 된다) $X$ 의 크기가 $\text{N} \times \text{D}$, $W$ 의 크기가 $\text{D} \times \text{C}$ 일 때 필자가 작성한 loss function의 코드는 다음과 같다. 

``` python 'SVM Loss with no loops'
S = X.dot(W)
ans_S = S[np.arrange(N), y]
margin = S - ans_S[:, None] + 1
loss = np.sum(np.maximum(0, margin))
loss /= N
loss += reg * np.sum(W*W) - 1
```

$X \cdot W$ 를 통해 $S$ 를 구하였으며, $S[i][j]$ 는 $x_i \cdot w_j$ 를 의미한다. `S[np.arrange(N), y]`를 해주면 $i$ 번째 칸이 $y_i$ 인 1-D vector를 얻을 수 있으며, 따라서 `margin` 값 ( $s_j - s_{y_i} + 1$ )을 계산할 때 Python Broadcasting을 통해 이용할 수 있다. 위 코드에서는 우선 $ j = y_i $ 인 경우까지 모두 고려하여 합쳤으며, $ j = y_i $ 인 경우는 항상 $\text{loss} = 1$ 이 추가되므로 마지막에 1을 빼 주었다. 

`np.maximum()`은 요소 하나하나 비교한다는 면에서, `np.max()`과 다르다는 것을 짚고 넘어가자!

gradient를 구하기 위해서는, 다음과 같은 Indicator function을 생각한다.

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

이 식들을 이용하여 $ L = {1 \over N} \sum_{i = 1}^{N} L_i $ 를 통해 total loss $L$ 을 구한다. 아래는 필자가 작성한 python no-loop 코드이다. (위의 코드를 이용한다)

``` python 'SVM Gradient with no loops'
binary = np.maximum(0, margin)
binary[binary>0] = 1
binary[np.arange(N), y] -= binary.sum(axis = 1)
dW = np.dot(X.T, binary)
dW /= N
dW += 2 * reg * W
```

Matrix의 shape을 통해 $X^T$와 `binary`를 곱하는 아이디어를 얻는다. **실제로 Matrix의 shape을 통해 유추하는 방식이 굉장히 자주 등장한다!!** Gradient의 크기는 $W$와 같아야 하며, 주어진 matrix들의 곱 만을 이용한다면 $X^T$와 $\text{N} \times \text{C}$의 크기를 가진 matrix을 곱해야 한다. 

`binary[i][j]`은 "$x_i^T$가 이 $j$에 대하여 몇 번 계산되는가?"를 의미한다. 위에 유도한 식에서 0보다 큰 margin 값들만 유지되었고, $j = y_i$인 경우에는 그 행에서 margin 값이 0보다 큰 애들의 개수만큼 빼 주었기 때문이다. 이를 바탕으로, $X^T$ 를 곱하는 정당성은 다음과 같이 생각해 볼 수 있다. 
$$
  \left[ 
  \begin{matrix}
    \vec{a} & \vec{b}
  \end{matrix}
  \right]

  \left[
    \begin{matrix}
      x & l   \\
      y & m   \\
    \end{matrix}
  \right]  =

  \left[
    \begin{matrix}
      \vec{a}x + \vec{b}y & \vec{a}l +\vec{b}m
    \end{matrix}
  \right]

$$

$X^T$ 를 $ \left[ 
  \begin{matrix}
    \vec{x_1^T} & \vec{x_2^T} & ... & \vec{x_N^T}
  \end{matrix}
  \right] $ 와 같이 보고 위와 비교해보면, 이해가 될 것이다!

## softmax.ipynb
Softmax에서의 loss function은 다음과 같다.

$$ L_i = -\log {e^{s_k} \over \sum_j e^{s_j}} $$

필자가 작성한 코드는 아래와 같다.

```python 'Softmax Loss with no loops'
exp_scores = np.exp(np.dot(X, W))
scores_row_sum = np.sum(exp_scores, axis = 1)
normalized_scores = exp_scores / scores_row_sum[:, None]
loss = np.sum(-np.log(normalized_scores[np.arange(N), y]))
loss /= N
loss += reg * np.sum(W*W)
```

각 행의 log 안쪽 분모 값을 `scores_row_sum`에 담았고, 전체 `exp_scores` 행렬의 각 행을 이 `scores_row_sum`으로 나누어 `normalized_scores`를 만들었다. (Python Broadcasting이 사용된다) 이후에는 -log을 씌우고 더해주면 된다.

Gradient를 구하기 위해서는 마찬가지로 $L_i$를 편미분해야 한다.

$$ j \neq y_i : \qquad {\partial L_i \over \partial w_j} = {e^{s_j} \over \sum_k e^{s_k}} \cdot x_i $$

$$ j = y_i : \qquad {\partial L_i \over \partial w_{y_i}} =  ( {e^{s_j} \over \sum_k e^{s_k}} - 1) \cdot x_i  $$

$ j = y_i $ 인 경우만 $x_i$를 한 번씩 더 빼주면 된다! 위 SVM의 경우와 마찬가지로 $X^T$와 곱하는 아이디어를 사용하여, 아래와 같이 해결할 수 있다.

```python
normalized_scores[np.arange(N), y] -= 1
dW = np.dot(X.T, normalized_scores)
dW /= N
dW += 2 * reg * W
```