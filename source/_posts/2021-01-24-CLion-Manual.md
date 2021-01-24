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