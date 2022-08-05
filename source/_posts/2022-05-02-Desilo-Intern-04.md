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
본 논문은 Google에서 진행한 흥미로운 연구인데, 요약하자면 **C++로 작성된 프로그램 자체를 동형암호화 할 수 있는 방안**에 대한 연구이다. 지금까지는 Data만을 암호화하였고, 한 번도 이를 벗어나는 생각을 해본 적이 당연히 없었던지라 처음 리드 개발자님께 논문을 소개받았을 때에는 아주 놀랐다. 프로그램 자체를 암호화하는게 된다고요? 딱 이 반응이었다. 

논문에서는 동형암호화에 관한 전문성이 부족한 개발자들이 동형암호를 활용한 개발을 하는 것이 가능케 하는 것이 목표라고 밝혔다. 실제로 필자도 동형암호 관련 개발을 위해 기본적인 수학적 개념을 이해하여야 했으며, 효율적인 parameter 설정에는 더 깊은 수준의 전문성을 요구하였다. 동형암호를 전혀 모르는 개발자도, C++로 작성된 코드에 이 (실험적인) FHE Transpiler를 적용하면 동형암호 field에서 작동하는 코드를 만들 수 있는 것이다!

프로젝트를 받은 시점이 21년 7월 중순이었고, 논문이 6월 중순에 나왔으니 나온 지 한 달이 지나지 않은 꽤나 따끈따끈한 논문이었다. 첫 번째 Task는 이 논문 자체를 이해하고 (Google 놈들이 무슨 짓을 해놓은건지 파악하라고 하셨다 ㅋㅋㅋ),  두 번째 Task는 회사에서 이 논문을 적용할 방안이 있는지 확인하는 것이었다. 

아래에 본 논문의 초록을 참고한다. 
> Fully homomorphic encryption (FHE) is an encryption scheme which enables computation on encrypted data without revealing the underlying data. While there have been many advances in the field of FHE, developing programs using FHE still requires expertise in cryptography. In this white paper, we present a fully homomorphic encryption transpiler that allows developers to convert high-level code (e.g., C++) that works on unencrypted data into high-level code that operates on encrypted data. Thus, our transpiler makes transformations possible on encrypted data.  
> Our transpiler builds on Google's open-source XLS SDK (https://github.com/google/xls) and uses an off-the-shelf FHE library, TFHE (https://tfhe.github.io/tfhe/), to perform low-level FHE operations. The transpiler design is modular, which means the underlying FHE library as well as the high-level input and output languages can vary. This modularity will help accelerate FHE research by providing an easy way to compare arbitrary programs in different FHE schemes side-by-side. We hope this lays the groundwork for eventual easy adoption of FHE by software developers. As a proof-of-concept, we are releasing an experimental transpiler (https://github.com/google/fully-homomorphic-encryption/tree/main/transpiler) as open-source software.


## 과정
논문을 읽으며 이해하는 것으로 시작하였다. 논문에서 사용된 Google XLS, Bazel 같은 tool들의 개념과 자주 사용해보지 않았던 TFHE (Torus FHE; AND, OR, NOT과 같은 Gate Operation들만을 사용하며 noise management 없이도 무한한 computation을 할 수 있다) 의 개념 등을 공부하며 이해하였으며, 앞선 프로젝트에서 '완벽히 새로운 분야에 머리 박으며 배우는 것'에 익숙해진 덕에 크게 어렵지 않았던 것 같다. 

본 Transpiler의 아이디어를 이해한 이후에는, Google 측에서 올려둔 experimental transpiler를 직접 돌려보며 여러 가지를 분석하였다. Transpiler를 C++ 코드에 직접 적용하고 Bazel을 이용하여 build하는 방법과 적용 가능한 C++ 코드들의 조건을 확인하였다. (실제로 Data-Independent한 코드들에게만 transpiler가 적용 가능하다. 예를 들자면, loop이나 array의 길이가 변수면 안 되며, recursion과 pointer가 포함된 코드도 적용 불가능하다.)

마지막으로는 제공된 Example codes들을 직접 돌려보며 걸리는 시간을 측정하였다. `num_opt_passes, Array size`의 두 변수를 직접 바꾸어가며 측정하였으며, 시간은 `abseil::Now()`을 이용한 Total Time과 `clock()`을 이용한 CPU Time을 모두 측정하였다. 그 결과, 아직 이 실험적인 transpiler는 현업에 직접 사용되기 어렵다고 판단하였다. 기본적인 a + b의 덧셈이 30초~1분 가량 걸렸으며, input string을 뒤집어 출력하는 예시 코드는 무려 28분이나 걸렸다. 두 변인에 대한 유의미한 결론도 내릴 수 있었다.

## 인턴 끝!
인턴 기간의 대부분을 독자적인 프로젝트 형태로 진행하였기에, 인턴 마지막 날에 수행한 두 프로젝트(PALISADE-Python, Google FHE Transpiler)를 모든 회사 사람들 앞에서 발표하게 되었다. 많은 분들께서 궁금증을 가지고 들어주시고, 질문해주셔서 감사하고 뿌듯한 경험이었다. 


프로젝트에 대해 더 자세히 알고 싶다면 아래 발표 파일을 참고할 것!  
[참고: 발표 파일](../../../../files/Desilo_Yeonsang_Projects.pdf)