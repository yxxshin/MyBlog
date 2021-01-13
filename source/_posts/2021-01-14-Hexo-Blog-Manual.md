---
layout: post
title:  "Hexo Blog Manual"
description: Commit Message, Hexo 명령어, 추가 사항들
date:   2021-01-14 15:00:00 +0900
categories: [ Manual ]
tags: [ manual ]
---

## Commit Message Type

- [FEAT] : 새로운 기능
- [BUG] : 버그 수정
- [UPDATE] : 업데이트
<!-- more -->
- [DOCS] : 문서
- [TEST] : 테스트
- [ETC] : 기타

## Hexo 명령어

- `hexo server` : 서버 실행 (http://localhost:4000)
- `hexo new page about` : `/source/about` 폴더에 `index.md` 파일이 생긴다. <mark>페이지 생성</mark> 
- `hexo new post 포스트명` : `/source/_posts` 폴더에 md 파일이 생긴다.  <mark>게시물 작성</mark> 
- `hexo generate` : <mark>정적 리소스 생성</mark> 
- `hexo deploy` : <mark>서버 배포</mark> 
- `hexo deploy --generate` 로 위의 두 과정을 동시에 할 수 있음
- 배포 정상적으로 됐는데 업데이트가 되지 않으면, `hexo clean`을 해준 후 다시 위의 코드 실행



## 기타 사항들

( 기존의 Manual 게시물(Jekyll Blog)에서 추가된 사항들 )

- <mark>Read More</mark> 버튼 생성: md 파일 중간에 `<!-- more -->` 적기
- cover와 thumbnail: md 파일 상단에 `cover: Pic URL`, `thumbnail: Pic URL` 로 적으면 된다