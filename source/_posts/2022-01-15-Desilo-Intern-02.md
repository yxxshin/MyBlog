---
layout: post
title: 'Desilo 인턴 돌아보기 - 02'
description: '2021.04.01 ~ 07.31 Desilo 스타트업 인턴 돌아보기 02'
date: '2022-01-15 15:00:00 +0900'
categories:

  - Memories
tags:
  - Desilo
toc: true
cover: /images/Desilo_Cover.png
thumbnail: /images/Desilo_Thumbnail.png
sitemap: false
noindex: true
---

## 02. 초기 Task들
첫 번째로 받은 초기 Task들을 간단하게 정리하고자 한다. 동형암호를 전문적으로 다루는 스타트업인 만큼 초반에는 동형암호와 그 라이브러리들에 대한 전반적인 지식이 요구되었다. 이후 회사의 HE (Homomorphic Encryption, 동형암호) Library 를 직접 만들어 배포하는 것에 기여하게 되었다. 대표님, 리드개발자님과 인턴을 위한 인터뷰를 진행하였을 때, [직전에 한 프로젝트](https://github.com/TeamWeathy/WeathyServer)이자 자신있는 파트 중 가장 현업과 직접적인 관련이 높았던 파트가 **서버 파트**였기 때문에, 자연스럽게 서버 관련 Task도 받게 되었다. 

<!-- more -->

## 동형암호란

**Homomorphic Encryption (동형암호)** 란, 암호화된 암호문들을 연산해도 그 성질을 잃지 않는 암호를 의미한다. f(x) + f(y) = f(x + y) 인 함수를 homomorphism 이라고 부르는 것을 떠올리면 이해하기 쉽다. a1, a2를 암호화했을 때 각각 c1, c2가 되었다면, c1 + c2를 복호화하면 a1 + a2 가 된다. (곱셈도 된다)

동형암호의 강력한 장점 중 하나는 데이터 연산 시 복호화 과정이 필요 없다는 점이다. 데이터셋을 머신러닝에 이용한다고 하자. 동형암호를 사용하지 않는다면, 

> 데이터셋 복호화 → 머신러닝 (데이터 조작) 하여 의미있는 결론 도출 → 데이터셋 암호화

의 과정을 거쳐야 한다. 복호화 과정이 불가피하게 최소 한 번은 필요한 것이다. 따라서 데이터를 조작하거나 의미 있는 결론을 얻어내기 위해서는 어쩔 수 없이 데이터 원문이 드러나게 된다. 이는 '해킹에 취약함'을 의미하기도 한다. 하지만 동형암호를 사용한다면, 복호화 하지 않고도 데이터셋에서 유의미한 결론을 얻어낼 수 있다! 

이는 우리 회사 [Desilo](https://desilo.ai/home)에게 반드시 필요한 기술이다. 정말 간단하게 설명하자면 Desilo는 회사들이 각자의 Dataset들을 업로드하거나, 다른 회사의 Dataset을 사고 팔 수 있는 플랫폼(B2B 서비스)을 만드는 것이 목표였다. (적어도 내 인턴 기간 동안에는) 사이트에 Dataset을 업로드 하면 동형암호로 자동 암호화 되며, 타 회사의 Dataset을 구매하면 사이트 내에서 데이터 조작을 할 권한을 얻게 된다. 동형암호의 특징을 이용하면 거래 및 데이터 조작 시에도 Dataset을 복호화할 필요가 없으며, 심지어 서버를 운영하는 Desilo 회사도 복호화할 수 없게 할 수 있다. (개인 암호 키를 본인 컴퓨터 하드웨어에만 저장해 두면, 서버 운영자를 포함한 그 누구도 복호화할 수 없다.) 이런 면에서 Dataset을 업로드하는 회사는 자회사 고객의 privacy 문제나, 회사 이익 측면 등에서 전혀 타격을 받지 않을 수 있다!

## Task 1-1 : Desilo HE Library

다양한 라이브러리들이 현재 동형암호를 지원해 주고 있는데, 가장 유명한 C++ 동형암호 라이브러리에는 Microsoft 사의 [SEAL](https://github.com/microsoft/SEAL) 과 Duality Technologies의 [PALISADE](https://gitlab.com/palisade/palisade-release)가 있다. (결국 본인은 인턴 기간 동안 이 두 라이브러리를 모두 잘 다루게 되었다)  

첫 번째 Task에서는 추후 회사에서 개발에 직접 사용할 Desilo HE Library를 만드는 것이었다. 동형암호의 연산은 SEAL 라이브러리를 이용하였는데, MVP 개발 시 SEAL을 매번 직접 이용하기 위해서는 개발자 모두가 SEAL과 동형암호에 대한 이해가 필요하기 때문에, 그리고 코드가 길어지고 더러워질 것이기 때문에 이를 방지하기 위해 API 형식으로 회사 자체 라이브러리를 만들고자 하였다. Input과 Output들이 회사의 MVP에 맞게 정해졌다. 본인은 Rotation Sum, Count, Filter 함수를 맡게 되었다.

Task 자체는 크게 어렵지 않았는데, 입사하고 첫 번째 Task인 만큼 처음에 정말 감을 많이 못 잡았던 것 같다. 회사의 환경(협업 툴 Gitlab를 비롯한 Notion 등등)에 적응하는 것을 떠나, 무엇보다 Task 해결의 시작이 막막하게 느껴졌다. 리드개발자님께 "어디서부터 어떻게 시작해야 할 지 정말 모르겠다"고 털어놓았을 정도였다.

Turning Point가 되었던 부분은 **이론 공부의 부족함**을 느끼고 처음으로 돌아가 더 꼼꼼히 공부한 것이었다. 초반에는 어서 Task를 해결하고 싶은 급한 마음에 동형암호와 라이브러리에 대한 심도 깊은 이해를 하지 않고 넘어갔다. 사실 어려운 정수론을 비롯한 수학이 배경에 있는 학문인지라 개인적으로 *어느정도까지 깊게 공부해야 하는가?*가 어려운 포인트였다. 동형암호의 간단한 응용만 하면 되는 Task에서 근본적인 원리까지 파헤치는 것은 불가능해 보이기도 했고, 비효율적일 것이기 때문이다. 처음 내린 결론은 '이정도만 이해해도 되겠지' 였지만 결국 이것이 발목을 잡고 괴롭혔다. 또 한 번 **기본기의 중요함**을 깨닫는 기회였다. 앞으로 내 인생에서도 이 질문은 자주 등장하여 나를 괴롭힐 것 같은데, 많은 경험을 통해 이를 익히는 것도 중요한 능력 중 하나가 될 것 같다. 어쨌든 **감이 잡히지 않으면 처음으로 돌아가 다시 공부해라**는 교훈을 얻었다.

또한, **질문**을 자주 한 것도 많은 도움이 되었다. 사실 입사 전까지 본인은 개인 프로젝트의 비중이 월등히 높았고, 도전적이고 어려웠던 경우는 대부분 스스로 하였다. 그래서 막혔을 때 누구에게 물어보기 보다는 스스로 구글을 뒤지거나, 직접 부딪혀가며 해결하였다. 그러나 집단 단위의 프로젝트, 특히 회사의 경우 이것이 정답이 아니라는 것을 깨달았다. 스스로 해결하는 문제해결 방법은 개인의 역량 키우기에는 도움이 될 수 있겠지만, 회사의 입장에서 이는 시간 낭비일 확률이 높다. 옆 사람이 똑같은 문제로 시간을 투자하여 해결했다면, 나는 답을 듣고 바로 다음 단계로 넘어가는 것이 회사에게는 이상적이고 효율적인 것이다. 인턴 첫 달 동안 나의 '질문'에 대한 인식이 많이 바뀌었던 것 같다. 인턴인 나에게도 정말 친절히, 기초부터 알려주었던 회사의 개발자 분들이 많은 긍정적인 영향을 준 것 같다 : )

결과론적으로 [SEAL](https://github.com/microsoft/SEAL) 과 [SEAL-Python](https://github.com/Huelse/SEAL-Python) 을 성공적으로 마스터하고, Library 함수도 성공적으로 완성하였다. **Pytest**을 이용하여 검증하라는 subtask도 함께 해결하였는데, 저번 [서버 프로젝트](https://github.com/TeamWeathy/WeathyServer) 때 한 API test와 유사한 형식을 지녀 비교적 빠르게 이해하고 완성할 수 있었다. (임의의 case들을 input으로 넣고 output을 확인한다는 원리는 같으므로) 추가적으로 **Monkeypatch**를 이용하는 방법도 공부하여 익힐 수 있었다.

## Task 1-2: User Scenario Test

Task 1-1에서 작성한 Library들을, 있을법한 Scenario에 적용하여 올바르게 동작함을 보여주는 Subtask 였다. Library와 마찬가지로 python(jupyter notebook)을 이용하였는데, python 환경에서 Data 처리를 쉽게 해주기 위해 **pandas** 라이브러리도 공부하여 익혔다! 

## Task 1-3: WebAPI

개인 프로젝트였던 다음 Palisade-Python (Pypalisade) 프로젝트 이전에 잠시 받았던 간단한 Task였다. MVP 웹사이트에 사용될 Register Local Dataset WebAPI를 작성하게 되었다. 웹 서버도 Python 기반의 **FastAPI**를 이용하고 있었는데, 동형암호로 암호화된 Dataset들이 굉장히 용량이 크기 때문에 비동기 처리를 위해 FastAPI를 채택한 것으로 기억한다. API 개발과 더불어 **MySQL**도 리눅스 환경에서 잘 다룰 수 있게 되었다.

## 마무리하며

초반에는 비교적 다양한 Task들을 하면서 다양한 skill들을 익힐 수 있었던 것 같다. 동형암호와 SEAL 라이브러리 공부가 꽤나 어려웠던 기억이 있다 - 동형암호의 이론 자체는 생각보다 쉬웠지만 직접 라이브러리를 사용하기 위해서는 생각해야 할 인자와 key가 꽤 많았고, 이 설정을 제대로 해주지 않으면 연산이 수행되지 않기 때문이었다. 애초에 Git은 굉장히 자유자재로 다룰 수 있었고, 이전의 협업 프로젝트들을 통해 협업 툴 사용법과 팁들은 알았기에 '회사에서의 협업' 부분에서는 큰 어려움 없이 적응할 수 있었다. 

위의 Task들은 입사한 2021.04.01 일자부터 05.31 까지 약 두 달에 걸쳐 수행하였다. 아예 새로 알게 된 동형암호의 개념부터, 기존에 다루었었던 백엔드 개발과 Python, C++ 문법 등등 전반적으로 기본기를 다지며 회사에 적응하고 스며든 시기였다. 