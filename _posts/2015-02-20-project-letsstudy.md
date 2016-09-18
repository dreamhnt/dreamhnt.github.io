---
layout: post
title:  "Let's study"
date:   2015-02-20
description: "스터디 모집 사이트"
category: Project
tags: [project, django]
comments: true
---

## 개요

---

- 기간 : 2015년 4월 ~ 3주
- 인원 : 1명
- 개발환경 : ubuntu(elementary os), nginx, Django, Mariadb

---

우연히 Ubuntu 기반의 Elementary OS Freya 의 출시 소식을 접하고 미려한 UI에 반해 설치 후 프로젝트를 진행하였다.
기존에 ubuntu를 여러번 사용해보긴 했지만 역시 환경 설정 부분에서 시간이 좀 걸렸지만 리눅스 계열은 삽질하는 맛이 있는 것 같다.
빠르고 편리하고 그나마 익숙한 Django 프레임워크를 이용하였으며 Mariadb와 함께 개발하였다.
ui 부분도 하드 코딩으로 개발했으며 아주 심플한게 특징이다.

## 기획 의도

스터디를 하고 싶어 스터디 멤바를 구하고 싶은데 스터디 카페에서 등업을 안해줘서 순간 큰 불편함을 경험하고, 스터디를 모집하는 사이트가 없어서 만들어본 간단한 사이트이다.

## 사이트 로드맵 & 기능

### 메인

![메인]({{ "/assets/img/project/ls1.png" }})
*메인화면*

- 사이트 처음 접속 시 메인화면으로 게시글의 리스트가 infinite scroll 로 보여진다.
- 분야, 지역별로 태그를 중복 클릭하여 게시글을 분류할 수 있다.
- 상단에는 검색 키워드에 해당하는 제목, 내용에 있는 글을 불러올 수 있다.

### 글쓰기

![글쓰기]({{ "/assets/img/project/ls2.png" }})
*글쓰기화면*

- 평범하다 못해 덜 평범할 수 있는 화면이다.
- 제목, 분야, 장소, 기간, 인원 등 필수 요소를 입력 받는다.

### 상세 화면

![상세화면]({{ "/assets/img/project/ls3.png" }})
*상세화면*

- 해당 글을 클릭 시 태그와 제목, 작성자의 정보, 내용이 보여지는 상세 화면이다.
- 참여 인원의 카운트에 따라 색상 그래프로 표시가 된다.

## 마무리

디자인이 미흡하며 네이버나 구글 지도를 이용한 장소의 표시 기능, 쪽지나 메시지 기능, 알림 기능 등 스터디를 모집함에 있어 필요한 기능들의 구현이 부족했다. 또한 반응형으로 개발하여 모바일에서도 편리하게 스터디를 모집할 수 있도록 개발이 필요하다.
기술적으로는 클릭된 분류별 정보를 DB에서 가져오는 부분과 데이터 포맷하는 부분에서 오래 걸렸고, 프론트쪽은 하드코딩으로 만들어보았다. 하드코딩으로 만들어서인지 sass, coffeescript 를 배워볼까 하는 생각이 여러번 들었고, 부트스트랩 같은 프레임워크 또한 저절로 생각이 났다.