---
layout: post
title:  "ERP 반응형웹"
description: "ERP 시스템의 부분적인 기능을 이용할 수 있는 반응형 웹 개발"
category: Project
tags: [project, spring, mssql]
comments: true
---

## 개요

---

- 기간 : 2016년 3월 ~ 5월
- 인원 : 1명
- 사용 기술 : spring framework, mssql, bootstrap, 전자정부프레임워크
- 사용 도구 : eclipse, svn
- 주요 기능 : ERP 시스템 업무

---

SAP 컨설팅 회사에서 웹개발자로 근무하면서 신규로 모바일 웹 개발 프로젝트를 혼자 진행하게되었다.

서버 기술로는 전자정부프레임워크를 사용해서 개발을 해야 했는데 처음 사용해 봐서 개발환경 세팅하기도 쉽지 않았다.
또한 회사에서 사용하던 스프링의 구조를 최대한 비슷하게 맞춰야 했는데 DB 연동도 안되고 수 많은 에러를 맞이하며 좌절을 겪어야 했다.
구글 선생님의 도움으로 우여곡절 끝에 세팅을 끝내고, 프론트엔드 개발에 대한 고민을 해야만 했다.
처음엔 리액트나 앵귤러 같은 기술을 사용하여 만들고 싶었지만 배우는데 또 시간이 소요되고, 유지보수를 할 때 다른 개발자들은 모르는 기술은
사용할 수 없고, 무엇보다 모바일 웹이 사용되는 규모가 작았다.

그 후 ERP 업무에 사용될 만한 UI가 괜찮은 오픈소스 템플릿을 찾으러 다녔고, 처음에는 버튼의 모서리 부분이 둥근 형식의 템플릿으로 개발을 했다.
팀장님한테 보여줬을 때 괜찮다고 했지만 내가 뭔가 마음에 들지 않아 새로운 템플릿을 또 찾기 시작했다.
최종적으로는 AdminLTE 라는 템플릿을 사용하게 되었는데 필요한 부분만 가져다 사용했고 심플하면서도 각진 모양이
ERP 업무에도 잘 어울렸다. 개발을 하면서 UI 나 디자인이 마음에 안드는 것들은 다른 라이브러리를 사용했고, SPA 개발에 중점을 두었다.
이것 저것 오픈소스들을 모아서 개발을 하는데 레고처럼 뗐다 붙였다 마음에 들때까지 하다보니 일관성 있는 사이트가 완성되었다.

사이트의 틀이 잡히고 실제로 개발에 들어가니 또 다른 문제가 생겼다.
ERP에서 사용되는 업무를 모바일 웹으로 사용하자니 수많은 항목들을 모바일 화면에 어떻게 보여줘야 할 지 고민을 해야했다. 일단 모바일에서 사용될 정보의 양을 줄였고, 탭바를 활용하고
테이블에 칼럼이 5이상이 넘어가면 행 클릭 시 상세정보가 나오게 변경했다. 그 밖에 정보의 표현에 있어 부딪히는 문제점들이 발생할 때마다 생각을 해야만 했고 그 과정들이 또 다른 즐거움으로 다가왔다.

처음엔 사수가 없어서 혼자 프로젝트를 진행하다보니 내가 잘하고 있는건지, 더 나은 방법은 없는지, 어디서 에러가 나올지도 모르는 불안감 속에서 개발을 했었다. 그렇지만 진행을
하다보니 다방면으로 검색을 해가며 해결 방법을 찾고 공부를 해가는 과정들이 나를 집착하게 만들었고, 그 만큼 만족할만한 결과물이 나왔을 때 희열과
기쁨이 배가 되었다. 분명 더 나은 기술이 있을테고 부족한 부분도 많을 테지만 내 범위 내에서 고민하고 최선을 다 했다는 것에 만족한다. 또한 실제로 여러 고객사에 이 소스를 상용화하면서 개발하다보니 아쉬운 부분도 점점 발견되고, 보완하는 과정도 일련의 재미였다. 특히 개발을 진행하면서 모듈화 하는 과정에서
작게 시작된 모듈이 점점 커지고 리팩토링 되어 갔는데 그러다 보니 모듈 소스에 대한 애착이 제일 강해진 것 같다. 

