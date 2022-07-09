---
layout: post
title: 'Desilo 인턴 돌아보기 - 03'
description: '2021.04.01 ~ 07.31 Desilo 스타트업 인턴 돌아보기 03'
date: '2022-04-11 15:00:00 +0900'
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

## 03. 첫 프로젝트
처음으로 받은 '프로젝트' 급의 과제였다. 첫 글에서 언급했듯 대표님과 리드 개발자님 모두 나의 최종 목표가 백엔드 개발자가 아니라는 것을 알고 계셨기에, 어떤 Task를 나에게 부여해야 회사와 나 모두에게 도움이 되는 방향인지에 대한 고민을 (정말 감사하게도) 많이 하셨다. 결론적으로 나에게 "개인 프로젝트"를 Task로 부여해 주었다. 

<!-- more -->

처음 Task를 받았을 때에는, 설렘에 심장이 쿵쿵 뛰었던 것 같다. 여태 개인 프로젝트는 학교에서 수업을 위한, 즉 나의 학점을 위한 용도였거나 개인 만족/실력 향상의 용도로밖에 쓰이지 않았는데, 내 개인 프로젝트가 회사(남)를 위해 쓰인다고? 내 실력을 제대로 발휘할 수 있는 절호의 기회라고 생각하였다. 최선을 다해 열심해 해보아야 겠다는 의지가 불타올랐다. ~~다가올 커다란 산은 생각하지 못한 채..~~

## Pypalisade Task 원문
**Create Palisade-Python**  
Duality Technologies have library called Palisade, released regularly here: [link](https://gitlab.com/palisade/palisade-release)

We can utilize the PALISADE when we can make a Python wrapper around the C++ library, as done here: [link](https://github.com/Huelse/SEAL-Python/blob/master/src/wrapper.cpp)

This involves:
- understanding CPython C APIs and how objects are transferred from C++ to Python environment
- understanding how we can do that using pybind11 - [link](https://pybind11.readthedocs.io/en/stable/basics.html)
- understanding the Python packaging process involving distutils - [link](https://docs.python.org/3/library/distutils.html)

Objective is to make relatively bug-free Python package that wraps PALISADE that we will publish on Github as desilo/Pypalisade.


## Task 보충설명 (21.07 기준)
Task에 대한 간단한 포인트들만 Q&A 형식으로 남겨두겠다.  

<u>Q1. SEAL과 같은 동형암호 라이브러리들을 잘 사용하고 있는데, 왜 PALISADE에 관심을 가지는가?</u>  
A1. PALISADE 라이브러리에서는 SEAL이 지원하지 않는 BinFHE(TFHE)나 Multiparty(Threshold HE) 등의 기능들을 지원한다. 실제로, PALISADE가 가장 실험적인 동형암호 라이브러리라는 평이 많았다.

<u>Q2. Duality Tehcnologies에서 PALISADE의 Python wrapper를 지원하는 것 같던데?</u>  
A2. 실제로 Demo 버전이 올라와 있긴 했으나, 몇 가지 문제점들이 있었다. 우선 소수의 기능들만 제공하는, 아이디어 제공용 라이브러리였다. (repo 설명 원문: This repository contains an example python 3 wrapper for PALISADE. It does not expose all functionality of PALISADE, rather it is an example of how to build a specific python application program using a python wrapper, Boost/python bindings and an installed PALISADE library. Check out the README in the repository for more details.)   
또한 정식 배포 버전이 아니므로 PALISADE의 업데이트에 맞추어 최신화가 될지도 미지수이다. 이는 회사에서 사용하는 목적에서 꽤나 치명적이다. 회사에서 직접 라이브러리를 만들게 되면 에러가 발생하더라도 직접 코드 수정을 하여 고칠 수 있지만, 가져다 쓰는 라이브러리가 에러를 뱉는다면 Duality 측에서 라이브러리를 건드려 직접 에러를 수정해 줄 때까지 우리는 할 수 있는게 없다.


## Task 수행 과정
첫 번째 과정으로는, 기본 지식 학습의 선행이 필수적이었다. 우선 다루어야 하는 **PALISADE 라이브러리**와 친해져야 했다. Duality Technologies에서 50페이지 가량의, 논문 형식의 [Manual](https://gitlab.com/palisade/palisade-release/-/blob/master/doc/palisade_manual.pdf)을 제공해 주어 많이 참고(10회독 가량..?)하였다. ~~그런데 오류 및 오타가 정말, 정말 많았다. 거의 두 페이지에 하나씩 있는 꼴이었다.~~ 기존의 SEAL과 같은 라이브러리에서 보지 못했던 Multiparty Homomorphic Encryption과 같은 개념들이 등장하여, 이해를 위해 [PALISADE Webinar](https://www.youtube.com/channel/UC1qByOsQina1rpZ8AGl5TZw/videos) 까지 들어 가면서 공부했던 기억이 난다. 

이후로는 wrapping 작업에 이용할 **pybind11** 툴을 익혔다. 공식 문서를 꼼꼼히 읽었고, 예제를 [직접 실행](https://github.com/yxxshin/Yeonsang_Study/tree/master/pybind11) 시켜 보며 알아갔던 것 같다. 가장 널리 보급된 Python인 CPython의 경우 C로 작성되어 있으므로 연관이 있다는 것은 사전에 인지하고 있었는데, C로 작성된 코드를 wrapping 작업을 통해 Python에서 구동시킬 수 있다는 점은 굉장히 신기했다. 

주요한 두 툴을 익힌 이후로는 이제 직접 PALISADE에 pybind11을 접목하여야 했다. PALISADE의 source code를 찬찬히 뜯어보았고 (생각보다 파일이 정말 정말 많았고 코드들도 기본 몇 백줄이었으며 구조 자체도 굉장히 복잡하게 되어 있어서 꽤나 오랜 시간이 걸렸다) 이미 pybinding된 선례인 [SEAL](https://github.com/microsoft/SEAL)과 [Huelse/SEAL-Python](https://github.com/Huelse/SEAL-Python/blob/master/src/wrapper.cpp)을 적극 참고하였다. (이 과정에서 사실상 SEAL과 SEAL-Python의 source code도 모두 익히게 되었다). 

PALISADE의 구조도 어느 정도 파악하였고, pybind11도 완벽하게 다룰 줄 알게 되어 자신 있게 pybinding을 시작하였다. 그러나, 시간이 지남에 따라 자연스럽게 정석적인 pybinding이 불가능함을 깨닫게 되었다. 


## 거대한 산 
위에서도 언급했듯 PALISADE는 구조가 기본적으로 복잡했다.
![PALISADE의 class 한 개의 구조](https://imgur.com/XR2t6Se.png)

또한 C++의 최신 문법을 포함한 다양한 문법이 이용되었는데, 이 중 깔끔하게 pybinding 되지 않는 문법들이 꽤나 많았다. 대표적으로는 smart pointers, templates, virtual functions, abstract classes 가 있었다. 이론상 pybinding이 불가능한 것은 아니었다. Custom smart pointer 들은 macro invocation을 이용한 transparent conversion을 이용하여 해결할 수 있다. Template의 경우는 입력 가능한 모든 타입을 하나씩 분해하여 직접 function이던, class이던 하나씩 수동으로 만들어 주면 된다. Virtual function을 포함한 abstract class들은 추가적인 "trampoline class"를 만들고 pybinding 하여 이 abstract class의 역할을 수행할 수 있었다. 

이론상 불가능한 건 없었다. 다만 수동적인 작업들이 많아 시간이 많이 요구되고 귀찮다는 점인데, 그게 **한 두개도 아니고 수백개가 넘는다는 사실**이다. 게다가 대부분의 class가 template 변수를 달고 다닌다는 것을 고려하면? 천 개가 넘을 것이다. 간단한 코드 작성 실험을 통해 한 class의 function을 사용하려면 그 class가 상속하는 모든 subclass들을 모두 pybinding하여 구현해야 한다는 것도 연이어 깨달았다. (어찌보면 당연하다) 옆 동네의 pybinding에 성공한 사례인 SEAL의 경우에는, PALISADE에는 떡칠이 되어 있는 template과 abstract class들이 전혀 없이 작성되어 있었으며 구조도 훨씬 간편했고, 코드들도 짧고 간결했다. 절망적이었다. 

*불가능하진 않지만 사실상 불가능하다.* 이 말이 나를 괴롭히기 시작했다. 설득력이 굉장히 부족했기 때문이다. 다른 사람에게는 '귀찮고 머리 아픈 작업이라 하기 싫어하는 건가?' 하는 냄새를 풍길 수도 있다. 어쩌면 무식하게 하나라도 먼저 시작했으면 인턴 기간 내에 어찌저찌 끝냈을 지도 모른다. (물론 그랬을 거 같진 않다. 최소 1년 반은 필요해 보였다.) 솔직하게는, 내가 성장하려고 하는 인턴인데 이 작업은 나의 성장과는 거리가 너무나 멀어 보이는 단순 노가다여서 꼭 이걸 해야되나.. 하는 심정이었다.  


## 새로운 IDEA
리드 개발자님께 어떻게 이 Task가 현실적으로 불가능함을 설명할지 고민을 거듭하던 찰나에, 새로운 IDEA를 떠올렸다. **꼭 직접 pybinding을 해야 할까?** 하는 의문에서 출발하였다. 내 Task는 PALISADE를 pybinding을 하는 것이지만, 결국 궁극적인 목표는 PALISADE을 python 환경에서도 사용할 수 있도록 해주는 것이 아닌가. 아래 사진과 같이 **새로운 라이브러리를 하나 만들어, 직접 PALISADE를 pybinding하는 것이 아닌 내가 만든 라이브러리를 pybinding하면 어떨까 하는 아이디어**를 떠올렸다. 

![NEW IDEA!](https://imgur.com/O4z8zMq.png)

나는 어떠한 C++의 구조와 문법들이 pybinding이 어려운지 알고 있으니, 새 라이브러리를 만들 때 이들이 포함되지 않도록 하면 된다. 라이브러리 자체에서 PALISADE를 import하여 사용하므로, 내 라이브러리에는 PALISADE의 말단 클래스 (직접 호출하여 사용하는 클래스) 들만 포함해도 되며 따라서 pybinding 해야 하는 코드 자체가 기하급수적으로 감소한다. 

## 해결!
생각해낸 아이디어로 몇 개만 구현하여 실행하였더니, 잘 동작하였다. 흐름을 타 쭈욱 코딩하였다. 추정되는 모든 결과들을 오류 없이 잘 뱉어내는 모습을 확인하였을 때, 그 순간과 그 뿌듯함은 1년 가까이 지난 지금도 잊을 수 없다. Manual부터 Example Codes까지, 하나의 프로젝트를 처음부터 끝까지 꼼꼼하게 완성한 경험이었다. (원본은 Desilo Git에 있으며, 본인의 Hidden repository에도 올려둔 상태이다)

![완성!](https://imgur.com/CzyRmLx.png)

## 돌아보기 

해냈다!

처음으로 어떠한 프로그램의 Source code를 찬찬히 뜯어봤다. 사실 github에 올라와있는 대부분의 프로그램들은 쓰고 사용법을 익히기에 바쁘지 source code를 뜯어볼 일이 없었는데, 이 과정 자체가 생각보다 많은 도움과 경험을 준 거 같다. 학교에서 배우지 못했던 최신 C++ 문법들이 생각보다 많았으며, 이 과정 자체만으로도 C++에 대한 개념이 많이 정리되었던 것 같다. (다른 사람의 코드를 많이 읽어보는 것의 중요성은 굳이 말하지 않아도 다들 알 것이다.)

결국 나는 하나의 라이브러리이자 프로그램을 만든 것이다! 이를 배포하여 다른 개발자들에게 사용법을 설명해주고, 직접 설치 및 실행했을 때 전혀 오류 없이 구동되는 것이 떨리면서도 너무나 뿌듯했다. 이런 맛에 프로그램을 만들어 배포하는 것인가?

설명서를 항상 읽어보는 나에게, 친절한 설명과 예시가 중요했다. 근데 사람들은 내 고민의 중간과정을 전혀 접하지 못하고 바로 결과물만 본다. 이를 의식하면서 설명서를 만들도록 했는데 역시나 쉽진 않았다. 의외로 Weathy 에서 Server API 만든 경험이 굉장히 많은 도움이 됐으며, 실제로 형식을 많이 참고하기도 하였다.

정석 방법(PALISADE를 직접 통째로 pybinding 하는 것)은 아니라 찜찜하긴 했지만, 그리고 회사에 실제로 이 코드가 쓰여 도움이 될진 모르겠지만 어쨌든 그 가능성을 봤다는 사실 자체가 중요하다고 생각한다. 나 한 명이 대표로 삽질을 하여, 아무도 가보지 않은 길을 내가 팠다. 내가 겪은 문제점들과 삽질한 포인트를 회사에서 인지하고 있는 것 자체만으로도 이득이 아닐까? ~~라고 스스로 믿고 있다~~ (아, 여기서 덧붙이자면 혼자 팠기 때문에 질문이 생겨도 물어볼 사람이 없었고, 이 부분이 어려움으로 다가오기도 했다.) 
 
자존감과 자신감이 올라간 것은 당연한 사실이다! 완벽히 완성하였을 때에는 잊을 수 없는 뿌듯함을 느꼈다. 이것이 나의 길임을 또 한 번 깨달은 순간이자, 너무나도 행복했던 순간이었다. 리드개발자님께서 '어딜 가더라도 뭐든지 잘할 거 같다'고 말해주신 건 덤이었다. 아직까지 내 마음을 울리는 최고의 칭찬이었다.
