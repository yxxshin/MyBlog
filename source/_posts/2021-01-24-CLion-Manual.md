---
layout: post
title:  "CLion Manual"
description: How I use CLion for C++
date:   2021-01-24 15:00:00 +0900
categories: [ Manual ]
tags: [ manual ]
---

아래 예시는 [이 폴더](https://github.com/yxxshin/Algorithm_Study) 를 기준으로 함.

파일의 `CMakeLists.txt`라는 파일이 있는데,  
컴파일 하고자 하는 파일이 바뀔 때마다 이 파일을 바꿔주어야 함

<!-- more -->

```c++
cmake_minimum_required(VERSION 3.16)
project(프로젝트이름)
// project(Baekjoon)

set(CMAKE_CXX_STANDARD 14)

add_executable(프로젝트이름 컴파일하려는파일이름)
// add_executable(Baekjoon Baekjoon_9186.cpp)
```

위의 `add_executable` 안에 컴파일 하려는 이름을 넣어주면 된다.

2021.01.29. 추가:  
GitHub에서 `cmake-build-debug` 나, `CMakeLists.txt` 등 CLion에서 C++ 파일을 빌드하는데 필요한 폴더/파일들을 untrack 하도록 하였다.  
새로운 환경이라면, 세팅을 해 놓은 뒤 필요한 cpp 파일만 pull 받는 것이 합리적! (대충 git pull 로 전체를 pull 해와도 실행이 안 된다는 뜻)