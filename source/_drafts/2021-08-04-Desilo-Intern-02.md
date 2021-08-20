---
layout: post
title: 'Desilo 인턴 돌아보기 - 02'
description: '2021.04.01 ~ 07.31 Desilo 스타트업 인턴 돌아보기 02'
date: '2021-08-04 15:00:00 +0900'
categories:
  - Memories
tags:
  - Desilo
toc: true
cover: /images/Desilo_Cover.png
thumbnail: /images/Desilo_Thumbnail.png
---

## 02. Pypalisade Project
본 프로젝트는 Desilo HE Library + User Scenario Test, WebAPI 작성의 가벼운 task들 이후에 처음으로 맡게 된 개인 단위의 프로젝트였다. C++로 작성된 Duality Technologies의 라이브러리 PALISADE를 Python 환경에서 이용할 수 있도록 Python Wrapper를 만드는 것이 Task이었다.

<!-- more -->

### Task 내용 원문 
Create Palisade-Python
Duality Technologies have library called Palisade, released regularly here: https://gitlab.com/palisade/palisade-release

We can utilize the PALISADE when we can make a Python wrapper around the C++ library, as done here: https://github.com/Huelse/SEAL-Python/blob/master/src/wrapper.cpp

This involves:
- understanding CPython C APIs and how objects are transferred from C++ to Python environment
- understanding how we can do that using pybind11 - https://pybind11.readthedocs.io/en/stable/basics.html
- understanding the Python packaging process involving distutils - https://docs.python.org/3/library/distutils.html

Objective is to make relatively bug-free Python package that wraps PALISADE that we will publish on Github as desilo/PyPalisade.