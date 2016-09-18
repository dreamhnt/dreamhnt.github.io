---
layout: post
title: "JSP 강좌 - JSP Fundamental#1"
description: "JSP 웹 프로그래밍 기본과 필수 이해 요소"
comments: true
category: Study
tags: [java, jsp]
---


## 웹 프로그래밍이란

### 개요

- HTML만으로 데이터가 실시간으로 변화하는 것을 처리하거나 저장하기에는 불가능
- 동적으로 변화하는 데이터를 처리하고 표시하기 위해서 개발된 것이 CGI, ASP, PHP, JSP
- 웹 프로그램은 기본적으로 클라이언트(Client) / 서버(Server) 방식

### 웹 어플리케이션

- 웹을 기반으로 실행되는 어플리케이션
- 웹 브라우저의 요청을 알맞게 처리하고 그에 대한 결과를 웹 브라우저에 전달

![웹 어플리케이션 구성요소]({{ "/assets/img/posts/jsp1.PNG" }})

#### URL

- Uniform Resource Locator
- [프로토콜]://[호스트][:포트][경로][파일명][.확장자][쿼리문자열]
- 예: http://www.google.com/search?hl=en&q=jsp&aq=f&oq=
  - 프로토콜: http
  - 호스트: www.google.com
  - 포트: 80 (http 프로토콜의 기본 포트)﻿
  - 경로: /search
  - 쿼리문자열: hl=en&q=jsp&aq=f&oq=
- URL은 웹 어플리케이션에 요청을 구분하기 위한 용도로 사용됨


### 서블릿과 JSP

자바를 만든 Sun에서 정한 웹 개발 표준
- 서블릿(Servlet) : 실행 코드 방식의 특징
- JSP(JavaServer Pages) : 스크립트 코드 방식의 특징

#### 실행코드 방식과 스크립트 방식

![실행코드방식과 스크립트방식]({{ "/assets/img/posts/jsp2.PNG" }})

#### JSP의 특징

- 자바 기반 스크립트 언어 - 자바의 기능을 그대로 사용 가능
- HTTP에 대한 클라이언트의 요청 처리/응답
- 웹 어플리케이션에서 결과 화면을 생성할 때 주로 사용
- 웹 컨테이너란 웹 어플리케이션을 실행할 수 있는 컨테이너로 JSP와 서블릿을 실행해 줌(Tomcat)

#### JSP를 사용하는 이유

- 자바 언어에 기반하기 때문에 플랫폼에 독립적 - 리눅스, 윈도우 등 운영체제에 상관없이 동작
- 자바 언어에 대한 깊은 이해 없이도 초기 학습 가능 - 스크립트 언어는 상대적으로 자바 언어보다 단순
- 스프링(Spring)이나 스트러츠(Struts)와 같은 프레임워크와 완벽하게 연동

## JSP 기본

### JSP 페이지 구성요소

JSP 페이지를 작성할 때는 다양한 요소들이 필요하며 다음과 같은 구성 요소를 익히는 과정이 JSP를 공부하는 과정이라 할 수 있다.

- 디렉티브(Directive)
- 스크립트 요소 - 스크립트릿(Scriptlet), 표현식(Expression), 선언부(Declaration)
- 기본 객체(Implicit Object)
- 표현 언어(Expression Language)
- 표준 액션 태그(Action Tag) 
- 커스텀 태그(Custom Tag)와 태그 라이브러리(JSTL)

### 디렉티브

- JSP 페이지에 대한 설정 정보를 지정
- 디렉티브 구문
  - <%@ 디렉티브이름 속성1="값1" 속성2="값2" ... %>
  - 예) <%@ page contentType = "text/html; charset=utf-8" %>
- 제공 디렉티브
  - page : JSP 페이지에 대한 정보를 지정
    - 문서의 타입, 출력 버퍼의 크기, 에러 페이지 등 정보 지정
  - taglib : JSP 페이지에서 사용할 태그 라이브러리를 지정
  - include : JSP 페이지의 특정 영역에 다른 문서를 포함

#### Page 디렉티브

page 디렉티브는 *JSP가 생성할 문서의 타입, 사용할 클래스, 버퍼 여부, 세션 여부* 와 같이 JSP 페이지에 대한 정보를 입력하기 위해서 사용된다.

#### Page 디렉티브의 주요 속성

![page디렉티브 속성]({{ "/assets/img/posts/jsp_paged.PNG" }})

```java
<%@ page contentType="text/html; charset=utf-8" %>
//웹컨테이너가 현재 파일을 읽어올 때 캐릭터 셋 설정(설정값 없으면 charset 값으로 셋팅)
<%@ page pageEncoding="utf-8" %>
//사용할 자바 클래스 import
<%@ page import="java.util.Date"%>
//공백문자제거
<%@ page trimDirectiveWhitespaces="true" %>
```

### 스크립트 요소

스크립트 요소는 JSP 프로그래밍에서 로직을 수행하는데 필요한 부분으로서, 스크립트 코드를 사용해서 프로그램이 수행해야 하는 기능을 구현할 수 있다.

- 요청을 처리하는 데 필요한 코드를 실행
  - 실시간으로 문서의 내용을 생성하기 위해 사용되는 것
- 동적으로 응답 결과를 생성하기 위해 사용
  - 사용자가 폼에 입력한 정보를 데이터베이스에 저장할 수 있음
  - 데이터베이스로부터 게시글 목록을 읽어와 출력할 수도 있음
  - 자바가 제공하는 다양한 기능들도 사용할 수 있음
- 스크립트 요소 세 가지
  - 스크립트릿(Scriptlet) : 자바 코드 실행 *<% 자바코드 %>*
  - 표현식(Expression) : 값을 출력 *<%= 값 %>*
  - 선언부(Declaration) : 자바 메서드(함수)를 정의 *<%! 메서드 %>*


```java
<%@ page contentType = "text/html; charset=utf-8" %>
//스크립틀릿
<%    
  int sum = 0;   
  for (int i = 1 ; i <= 10 ; i++) {       
    sum = sum + i;   
  }
%>
1 부터 10까지의 합은 <%= sum %> 입니다. //표현식

//선언부
<%!   
  public int multiply(int a , int b) {        
  int c = a * b;       
  return c;   
  }
%>
```

JSP를 사용하여 웹 어플리케이션을 해발할 때에는 선언부를 거의 사용하지 않는다. 그 이유는 선언부에서 정의한 메서드롸 같은 기능을 제공하는 클래스를 작성해서 그 클래스를 스크립트릿이나 표현식에 사용하는 것이 일반적이기 때문이다.

### request 기본 객체

기본 객체는 웹 프로그래밍에 필요한 기능을 제공하며 JSP에서 별도 선언 없이 사용이 가능하다.

*주요 기본 객체*

- request : 요청 정보를 구할 때 사용
- response : 응답과 관련된 설정(헤더, 쿠키 등)시 사용
- out : 직접 응답을 출력할 때 사용
- session : 세션 관리에 사용

request 기본 객체는 JSP 페이지에서 가장 많이 사용되는 기본 객체로서 웹 브라우저의 요청과 관련이 있다.

*주요 기능*

- 클라이언트(웹 브라우저)와 관련된 정보 읽기 기능
- 서버와 관련된 정보 읽기 기능
- 클라이언트가 전송한 요청 파라미터 읽기 기능
- 클라이언트가 전송한 요청 헤더 읽기 기능
- 클라이언트가 전송한 쿠키 읽기 기능
- 속성 처리 기능

#### request 객체의 주요 정보 제공 메서드

![request 정보 제공 메서드]({{ "/assets/img/posts/jsp_request.PNG" }})

#### 요청 파라미터 처리

JSP에서 가장 많이 사용하는 기능 중의 하나는 웹 브라우저 폼에 입력한 값을 처리하는 것이다.

![request 파라미터 읽는 메서드]({{ "/assets/img/posts/jsp_request2.PNG" }})

JSP에서 파라미터 로딩 시 인코딩 지정 필요(POST 방식)

```java
<%    
request.setCharacterEncoding("euc-kr");   
String name = request.getParameter("name");
%>
```

*톰캣에서 GET 방식 파라미터를 위한 인코딩*

```xml
//[톰캣설치디렉토리]/conf/server.xml

<Connector port=“8090” protocol=“HTTP/1.1”                 
connectionTimeout=“20000”                 
redirectPort=“8443”                 
useBodyEncodingForURI=“true” />//request.setCharacterEncoding(“utf-8”); 이 적용됨

<Connector port=“8090” protocol=“HTTP/1.1”                 
connectionTimeout=“20000”                 
redirectPort=“8443”                 
URIEncoding=“utf-8” /> //request.setCharacterEncoding(“utf-8”); 은 적용되지 않음
```

### response 기본 객체

response 기본 객체는 request 기본 객체와는 정반대의 기능을 수행한다. request 기본 객체가 웹 브라우저가 전송한 요청 정보를 담고 있는 반면에 response 기본 객체는 웹 브라우저에 보내는 응답 정보를 담는다.

주요 기능

- 헤더 정보 입력
- 리다이렉트 처리

### 헤더 정보

response 기본 객체 헤더 설정 메서드

![response 메서드]({{ "/assets/img/posts/jsp_response.PNG" }})

```java
<%    
  response.setHeader(“Pragma”, “No-cache”);    
  response.setHeader(“Cache-Control”, “no-cache”);    
  response.addHeader(“Cache-Control”, “no-store”);
  response.setDateHeader(“Expires”, 1L);%>
```

### 리다이렉트 처리

리다이렉트 기능이란 웹 서버가 웹 브라우저에게 다른 페이지로 이동하라고 지시하는 것을 의미한다. 웹 브라우저는 실질적으로 요청을 두번 하게된다.

![리다이렉트 흐름]({{ "/assets/img/posts/jsp_redirect.PNG" }})

```
<%@ page import="java.sql.*" %>
<%
  //JSP 페이지에 필요한 코드 실행
  ...
  ...
  response.sendRedirect("이동할페이지");
%>
```

## 필수 이해 요소

### JSP의 처리과정

![jsp 처리과정]({{ "/assets/img/posts/jsp_logic.PNG" }})

JSP 페이지를 요청할 때에는 JSP를 직접 실행하는 것이 아니라, JSP를 자바 소스 코드로 변환을 한 뒤 컴파일 해서 생성된 서블릿을 실행하는 것이다.
여기서 JSP 페이지를 자바 코드로 변경하는 단계를 *변환(translation) 단계* 라고 하며, 자바 코드를 서블릿 클래스로 변경하는 단계를 *컴파일 단계* 라고 한다.
**즉, JSP를 실행한다는 말은 곧 JSP 페이지를 컴파일 한 결과인 서블릿 클래스를 실행한다는 의미가 된다.**

### 출력 버퍼와 응답

JSP 페이지는 생성된 결과를 곧바로 웹 브라우저에 전송하지 않고, **출력 버퍼** 라고 불리는 곳에 임시로 출력 결과를 저장했다가 한번에 웹 브라우저에 전송한다.

출력 버퍼의 장점

- 데이터 전송 성능 향상
- 버퍼가 다 차기 전까지 헤더 변경 가능
- JSP 실행 도중 버퍼를 비우고 새 내용 전송 가능

#### page 디렉티브의 buffer 속성

```java
<%@ page buffer="8kb" autoFlush="true" %> : 버퍼 크기를 8Kbyte로 지정
<%@ page buffer="none" %> : 버퍼 사용 안 함
//none일 경우에는 <jsp:forward> 사용 못하고, 출력 내용 취소 불가
```
`autoFlush` - 버퍼가 다 찼을 때 처리 방식 지정
- true - 버퍼가 다 찼을 경우 버퍼를 플러시하고 계속해서 작업을 진행한다.
- false - 버퍼가 다 찼을 경우 예외를 발생시키고 작업을 중지한다
- 작성 안하면 default 로 true 이다.
