---
layout: post
title: '군대 싸지방에서 블로그 관리하기 (Hexo)'
description: '아무것도 없는 환경에서 Hexo 블로그 관리하기'
date: '2022-06-26 15:00:00 +0900'
categories:
  - Manual
tags:
  - manual
toc: true
noindex: false
sitemap: true
---

싸지방 컴퓨터는 예전 중고등학교 시절 컴퓨터실의 컴퓨터들과 같이 껐다 키면 모든 파일이 삭제되고 초기화된다. 따라서 Github을 이용한 Hexo 블로그를 이용하는 본인과 같은 경우에는 매번 필요한 프로그램들을 설치하고 설정들을 관리하여야 했다. 이 일련의 과정을 정리해두었다.

<!-- more -->


## 블로그 관리 세팅

1. Download and install **Node.js** (from [here](https://nodejs.org/en/download/))
2. Download and install **GIT** (from [here](https://git-scm.com/download/win))
3. Download and install **Visual Studio Code** if you need a markdown editor (from [here](https://code.visualstudio.com/))
4. **Git Clone** your blog repo (from [here](https://github.com/yxxshin/MyBlog))
``` bash
git clone https://github.com/yxxshin/MyBlog
```
5. Run **npm install** at the blog repo
``` bash
npm install
```
6. **Install Hexo**
``` bash
npm install -g hexo-cli
```
7. Set your **GIT default** account
``` bash
git config --global user.email "samshin3910@snu.ac.kr"
git config --global user.name "yxxshin"
```


끝났다! Hexo 사용법은 [기존의 블로그 게시물](https://yxxshin.github.io/2021/01/14/2021-01-14-Hexo-Blog-Manual/)을 참고하면 된다.