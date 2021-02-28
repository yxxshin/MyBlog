---
layout: post
title:  "Hexo Blog Manual"
description: Commit Message, Hexo 명령어, 추가 사항들
date:   2021-01-14 15:00:00 +0900
categories: [ Manual ]
tags: [ manual ]
toc: true
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
- `hexo new draft 포스트명` : `/source/_drafts` 폴더에 md 파일이 생긴다. <mark>초안 작성</mark>
  - `/source/_drafts` 폴더의 md들은 서버 배포해도 블로그에 표시되지 않는다.
  - 포스팅이 끝나면 직접 `_posts` 로 옮겨주거나 `hexo publish [layout] [파일명]` 으로 옮겨줄 수 있다. (layout 생략하면 기본적으로 post)
  - 로컬에서 draft를 테스트하고 싶다면 `hexo server --draft` 를 이용한다.
- `hexo generate` : <mark>정적 리소스 생성</mark> 
- `hexo deploy` : <mark>서버 배포</mark> 
- `hexo deploy --generate` 로 위의 두 과정을 동시에 할 수 있음
- 배포 정상적으로 됐는데 업데이트가 되지 않으면, `hexo clean`을 해준 후 다시 위의 코드 실행



## 기타 사항들

( 기존의 Manual 게시물(Jekyll Blog)에서 추가된 사항들 )

- <mark>Read More</mark> 버튼 생성: md 파일 중간에 `<!-- more -->` 적기
- cover와 thumbnail: md 파일 상단에 `cover: Pic URL`, `thumbnail: Pic URL` 로 적으면 된다
- md 파일 상단에 `toc: true`를 추가해 주면, markdown의 H1 ~ H6 에 맞게 <mark>CATALOGUE</mark>를 자동으로 생성해 준다!

## Hexo Tag Plugins

### Block Quote
- 인자가 없는 일반 인용
```
{% blockquote %}
This is a quote
{% endblockquote %}
```
{% blockquote %}
This is a quote
{% endblockquote %}

- 책 인용
```
{% blockquote [Author], [BookName] %}
This is a book quote
{% endblockquote %}
```
{% blockquote Author, BookName %}
This is a book quote
{% endblockquote %}

- 사이트 인용
```
{% blockquote [Author] [URL] [SiteName] %}
This is a site quote
{% endblockquote %}
```

{% blockquote Author https://google.com SiteName %}
This is a site quote
{% endblockquote %}

### Code Block
```
{% codeblock [title] [lang:language] [url] [link text] [options] %}
codes
{% endcodeblock %}
```
Option들은 [여기](https://hexo.io/ko/docs/tag-plugins.html#Code-Block) 에서 참고

### Backtick Code Block
위의 Code Block과 마찬가지로, language 뒤에 title, url, link text 를 추가할 수 있다!

### Youtube
- 비디오 추가
```
{% youtube [VideoId] %}
```

- 플레이리스트 추가
```
{% youtube [PlaylistId] 'playlist' %}
```

### Include Posts
```
{% post_link [post title] '[custom text]'%}
```

### Spoiler
```
{% spoiler "제목 문구" %}
본문 내용
{% endspoiler %}
```