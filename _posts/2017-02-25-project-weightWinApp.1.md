---
layout: post
title:  "윈도우 저울 응용프로그램"
description: "저울 및 프린터를 연동하여 데이터 생성 및 조회하는 윈도우 응용프로그램 개발"
category: Project
tags: [project, mfc, mssql]
comments: false
---

## 개요

---

- 기간 : 2016년 9월 ~ 11월
- 인원 : 2명
- 사용 기술 : MFC, MS-SQL, ZPL, DPL
- 사용 도구 : Visual Studio
- 주요 기능 : 제품의 무게를 측정하고 출력하며 데이터를 저장 및 조회

---

SAP 컨설팅 회사에서 근무하면서 돼지 도축 및 가공하는 기업으로 파견을 나가게 되었다. 

그곳에서 내 담당은 웹개발(주), 서버 개발(주), SAP AddOn(부) 개발 이었다. 도축 회사이기 때문에 부분육들의 무게를 측정하고
라벨지에 출력, 데이터 저장이 실시간으로 이루어지는 윈도우 응용프로그램 개발도 필요한 상황이었다. 처음에 이 프로그램의 개발은 다른
개발자가 진행하였는데 데드라인은 다가오는데 프로그램이 오류가 계속나면서 진행이 진척되지 않자 팀장님이 나에게 맡아서 개발해보라하셨는데
난 MFC 개발은 해본 적도 없고 당시 나의 업무로 많이 바쁜 상황이어서 거절했다. 그러다 다시 요구하셔서 소스라도 살펴볼려 했더니
클래스 파일 하나에 25000줄 정도의 코드가 있는걸 보고 경악을 금치 못하고 또 거절했다. 하지만 해당 개발자가 다른걸 신규 개발할 상황이 
생겨서 내가 할 수 밖에 없었다. 

이 프로그램의 기능은 작업 지시를 조회 후 제품이 들어오는대로 무게를 측정하고 동시에 데이터를 생성하고 라벨지를 출력하여 상자에 붙일 수 있게 하는 것이었다. 그렇게 하루에 3~4천 건 정도의 데이터가 쌓이게 된다. 프로그램이 복잡하거나 많은 기능이 들어간 건 아니어서 라인 수가 많은 
이유를 알 수가 없었고 리팩토링이 시급해보여서 거의 다 뜯어 고칠 수 밖에 없었다. 일단 내가 MFC 경험이 없어서 필요한 기능만 빠르게 익히고 코어 소스를 분석했는데 너무 심하게 중복된 코드가 많았다. 사실 그냥 개발할 수도 있었지만 25000줄의 파일을 열때마다 스트레스 받고 보기도 싫어서 최대한 줄이는 걸 일단 목표로 삼았다. 그렇게 리팩토링 되어서 2000줄까지 줄일 수 있었고 뿌듯한 마음보단 허탈한 마음이 더 컸던 것 같다. 코드가 정리가 되니 본격적으로
프로그램을 보완해 나가기 시작했다. 저울과 라벨지프린터에 연동되는 시리얼포트 통신에서 좀 애를 먹었고, 디스플레이에 무게가 소수점 밑이 불규칙적으로 짤려나가는 이유는 찾기도 힘들었는데 해결할 수 있었다.

저울로 무게를 측정하고 라벨지프린터에 바로 출력이 되어야 하는데 프린터 드라이버를 설치하여 연동을 하니 출력 속도가 너무 느렸다. 그래서
찾아본 결과 프린터 언어로 바로 통신 포트에 쏴주는 방식이 빠르다는 걸 알게 되었다. 사실 프린터 언어가 있는 줄도 몰랐는데 이 도축 회사는
사용하는 프린터 회사도 달라서 ZPL, DPL 두 가지 언어를 배워야했고, 필요한 부분만 배웠는데도 한글 출력문제도 생기고 시간이 꽤나 소요되었다.
우여곡절 끝에 개발을 완료하고 실무에 사용해보니 프로그램이 죽는 현상도 생기고 몇 가지 에러가 발생되었다. 실무에서 이 프로그램을 사용하고 에러가 발생 시 즉각 수정하지 않으면 기존의 프로그램으로 다시 데이터를 생성해야 하는 상황이어서 에러를 수정 못하면 이쪽 직원들의 불만도
많았고 나도 부담감에 스트레스가 쌓일 수 밖에 없었다. 다행히 몇 차례 보완을 해가며 프로그램을 완성할 수 있었다. 

새로운 분야를 배우는 걸 좋아하던 나지만 관심이 안가는 분야에 대한건(MFC, ZPL) 처음에 스트레스로 다가왔다. 그렇지만 해야만 했고 또 
이런 경험도 쌓이면 소중한 자산이 될 것이라 생각하고 마음을 다 잡고 개발을 할 수 있었다. 하지만 내가 이 프로그램 담당자가 되어 실무에
적용하고 보완하는 과정에서 부담감이 크게 다가왔고, 테스트 때 최대한 오류를 방지하려 했지만 라이브하면 꼭 뜻하지 못한 곳에서 에러가 발생하곤 했다. 심적으로 힘든 프로젝트였지만 완성된 프로그램을 사람들이 사용하는걸 보니 뿌듯한 마음이 더 컸고, 부분 부분 요구사항들을 만들어 주고 고맙다는 말을 들었을 때 힘이 생기고 보람찼다. 그리고 추가 개발사항들이 점점 많아지고 유지보수를 하면서, API나 아키텍처의 분석과 설계가 정말 중요함을 다시 한 번 느꼈다.


