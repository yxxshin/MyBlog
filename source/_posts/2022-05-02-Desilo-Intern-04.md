---
layout: post
title: 'Desilo 인턴 돌아보기 - 04'
description: '2021.04.01 ~ 07.31 Desilo 스타트업 인턴 돌아보기 04'
date: '2022-05-02 15:00:00 +0900'
categories:
  - Memories
tags:
  - Desilo
toc: true
cover: /images/Desilo_Cover.png
thumbnail: /images/Desilo_Thumbnail.png
noindex: true
sitemap: false
---

## 04. 두 번째 프로젝트: Google FHE Transpiler Research
첫 번째 프로젝트 (PALISADE-Python) 를 수행한 후, 일주일 안에 다음 프로젝트를 받게 되었다. 이후 2~3주동안 이 프로젝트를 수행하다가 인턴을 마치게 되었다. 

<!-- more -->

## Task 설명
본 프로젝트는 Google의 논문 [*A General Purpose Transpiler for Fully Homomorphic Encryption*](https://research.google/pubs/pub50428/)에 대한 연구 및 더 나아가 회사에 적용할 방안이 있는지 확인하는 프로젝트였다. 

## Google FHE Transpiler 논문
본 논문은 Google에서 진행한 흥미로운 연구인데, 요약하자면 **C++로 작성된 프로그램 자체를 동형암호화 할 수 있는 방안**에 대한 연구이다. 지금까지는 Data만을 암호화하였고, 한 번도 이를 벗어나는 생각을 해본 적이 당연히 없었던지라 처음 리드 개발자님께 Task를 소개받았을 때에는 아주 놀랐다. 프로그램 자체를 암호화하는게 된다고요? 딱 이 반응이었다. 

논문에서는 동형암호화에 관한 전문성이 부족한 개발자들이 동형암호를 활용한 개발을 하는 것이 가능케 하는 것이 목표라고 밝혔다. 실제로 필자도 동형암호 관련 개발을 위해 기본적인 수학적 개념을 이해하여야 했으며, 효율적인 parameter 설정에는 더 깊은 수준의 전문성을 요구하였다. 동형암호를 전혀 모르는 개발자도, C++로 작성된 코드에 이 (실험적인) FHE Transpiler를 적용하면 동형암호 field에서 작동하는 코드를 만들 수 있는 것이다!

프로젝트를 받은 시점이 21년 7월 중순 경이였고, 논문이 6월 중순에 나왔으니 꽤나 따끈따끈한 논문이었다. 첫 번째 Task는 이 논문 자체를 이해하고 (Google 놈들이 무슨 짓을 해놓은건지 파악하라고 하셨다 ㅋㅋ),  두 번째 Task는 회사에서 이 논문을 적용할 방안이 있는지 확인하는 것이었다. 

## 수행 과정


## 해결
