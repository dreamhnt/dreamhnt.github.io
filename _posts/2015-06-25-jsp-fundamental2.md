---
layout: post
title: "JSP 강좌 - JSP Fundamental#2"
description: "기본객체와 영역, 에러처리, 쿠키, 세션"
comments: true
category: Study
tags: [java, jsp]
---



## 기본 객체와 영역

**기본객체(Implicit Object))**

![jsp 기본 객체]({{ "/assets/img/posts/jsp_object.PNG" }})

위의 기본 객체 중에서 Exception 기본 객체를 제외한 나머지 8개 기본 객체는 모든 JSP 페이지에서 사용할 수 있으며, Exception 기본 객체는 에러 페이지에서만 사용할 수 있다.

### out

- JSP 페이지가 생성하는 모든 내용은 out 기본 객체를 통해서 전송.
- 웹 브라우저에 데이터를 전송하는 출력 스트림으로서 JSP 페이지가 생성한 데이터를 출력.
- 복잡한 if-else 사용시 out 기본 객체 사용하면 편리.
- print(), println(), newLine()

### pageContext

pageContext 기본 객체는 하나의 JSP 페이지와 1-1 매핑되는 객체로서 다음과 같은 기능을 제공한다.

- 다른 기본 객체 구하기
- 속성 처리학기
- 페이지의 흐름 제어
- 에러 데이터 구하기

![jsp pageContext 메서드]({{ "/assets/img/posts/jsp_page.PNG" }})

### application

특정 웹 어플리케이션에 포함된 모든 JSP 페이지는 하나의 application 기본 객체를 공유하게 된다.

#### 초기화 파라미터 설정

```xml
//파일명: web.xml
<context-param>
    <description>파라미터 설명(필수 아님)</description>
    <param-name>파라미터이름</param-name>
    <param-value>파라미터값</param-value>
</context-param>
```
web.xml 파일에 위와 같이 초기화 파라미터를 추가하게 되면, application 기본 객체에서 제공하는 메서드를 사용하여 초기화 파라미터를 JSP 페이지에서 사용할 수 있게 된다.

![jsp application 초기화 메서드]({{ "/assets/img/posts/jsp_application.PNG" }})

웹 어플리케이션 초기화 파라미터는 주로 초기화 작업에 필요한 설정 정보를 지정하기 위해 사용된다. 예를 들면 데이터베이스 연결과 관련된 설정 파일의 경로, 로깅 설정 파일, 주요 속성 정보를 담고 있는 파일의 경로 등을 지정할 때 사용된다.

#### 자원 구하기

![jsp application 자원 구하기]({{ "/assets/img/posts/jsp_application2.PNG" }})

### 기본 객체와 영역



## 에러 처리

기본적으로 출력되는 상세한 에러 페이지 화면은 사용자로 하여금 사이트에 대한 신뢰를 떨어뜨리기 때문에, 사용자에게 친숙한 에러 화면을 보여주어야 한다.
JSP에서는 JSP 페이지를 처리하는 도중에 Exception이 발생할 경우 에러 화면 대신 지정한 JSP 페이지를 보여줄 수 있는 기능을 제공하고 있다.

### 에러 페이지 지정과 작성

```java
//파일명: main.jsp
//에러 페이지 지정
<%@ page errorPage = "error.jsp" %>
```

page 디렉티브의 errorPage 속성을 사용해서 에러 페이지를 지정하면, 에러가 발생할 때 지정한 에러 페이지를 사용하게 되고, 에러 페이지에 해당하는 JSP 페이지는 page 디렉티브의 isErrorPage 속성의 값을 true로 지정해 주어야 한다.

```java
//파일명: error.jsp
//에러 페이지 작성
<%@ page isErrorPage = "true" %>
에러 타입: <%= exception.getClass().getName() %>
에러 메시지: <%= exception.getMessage() %>
에러 추적 메시지: ﻿<%= exception.printStackTrace() %>
```

### 응답 상태 코드별로 에러 페이지 지정

```xml
//파일명: web.xml
<web-app ...>
    ...
    <error-page>
        <error-code>404</error-code>
        <location>/error/error404.jsp</location>
    </error-page>
    ...
</web-app>
```

### 에러 타입별 에러 페이지 지정

```xml
<web-app ...>
    ...
    <error-page>
        <exception-type>java.lang.NullPointerException</exception-type>
        <location>/error/errorNullPointer.jsp</location>
    </error-page>
    ...
</web-app>
```

### 에러 페이지 우선 순위

1. page 디렉티브의 errorPage 속성에서 지정한 에러 페이지를 보여준다.
2. JSP 페이지에서 발생한 예외 타입이 web.xml 파일의 <exception-type>에서 지정한 예외 타입과 동일한 경우 지정한 에러 페이지를 보여준다.
3. JSP 페이지에서 발생한 에러 코드가 web.xml 파일의 <error-code>에서 지정한 에러 코드와 동일한 경우 지정한 에러 페이지를 보여준다.
4. 아무것도 해당되지 않을 경우 웹 컨테이너가 제공하는 기본 에러 페이지를 보여준다.

## 쿠키(Cookie)

웹 브라우저는 파라미터를 사용해서 웹 서버에 정보를 전달한다. 반대로 웹 서버가 웹 브라우저에 정보를 전달하는 방법이 있는데, 그것이 쿠키를 사용하는 것이다. 웹 서버와 웹 브라우저는 쿠키를 사용해서 서로 필요한 값을 공유하며 상태를 유지할 수 있다.
쿠키는 웹 브라우저가 보관하고 있는 데이터로서, 웹 브라우저가 웹 서버에 요청을 보낼 때 쿠키를 함께 전송한다. 쿠키는 웹 서버와 웹 브라우저 양쪽에서 생성할 수 있으며, 웹 서버는 웹 브라우저가 전송한 쿠키를 사용하여 필요한 데이터를 읽어올 수 있다.

![jsp 쿠키]({{ "/assets/img/posts/jsp_cookie.PNG" }})

### 쿠키의 구성

#### 구성 요소

- 이름 - 각각의 쿠키를 구별하는 데 사용되는 이름
- 값 - 쿠키의 이름과 관련된 값
- 유효시간 - 쿠키의 유지 시간
- 도메인 - 쿠키를 전송할 도메인
- 경로 - 쿠키를 전송할 요청 경로

#### 쿠키 이름의 제약

- 쿠키의 이름은 아스키 코드의 알파벳과 숫자만을 포함할 수 있다.
- 콤마(,), 세미콜론(;), 공백(' ') 등의 문자는 포함할 수 없다.
- '$'로 시작할 수 없다.

쿠키의 값이 알파벳과 숫자가 아닌 바이너리 값을 포함하고 있는 경우 BASE64 인코딩으로 처리해주어야한다.
쿠키의 값은 공백, 괄호, 등호기호, 콤마, 콜론, 세미콜론 등의 문자를 포함할 수 없기 때문에 이들 값을 포함하기 위해서는 반드시 인코딩 처리를 해주어야한다.
쿠키는 값으로 한글과 같은 문자를 가질 수 없기 때문에 인코딩 해주어야 한다.

### 쿠키 CRUD

#### 쿠키 생성

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.net.URLEncoder" %>
<%
	Cookie cookie = new Cookie("name", URLEncoder.encode("홍만이", "UTF-8"));
	response.addCookie(cookie);
%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>쿠키생성</title>
</head>
<body>
<%= cookie.getName() %> 쿠키의 값 = "<%= cookie.getValue() %>"
</body>
</html>
```

#### 쿠키 읽기

```java
// 쿠키 읽기
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.net.URLDecoder" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>쿠키 읽기</title>
</head>
<body>
<%
	Cookie[] cookies = request.getCookies();
	if(cookies != null && cookies.length>1){
		for(int i=0;i < cookies.length; i++){
%>
<%= cookies[i].getName() %> =
<%=URLDecoder.decode(cookies[i].getValue(), "UTF-8") %>
<% 		}
	} else {
%>
쿠키가 존재하지 않습니다.
<%
	}
%>
</body>
</html>
```

#### 쿠키 변경

쿠키 값을 변경하기 위해서 별도의 메서드가 제공되는 것이 아니라 같은 이름의 쿠키를 새로 생성해서 응답 데이터로 보내주면 된다.

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.net.URLEncoder" %>
<%
Cookie[] cookies = request.getCookies();
if (cookies != null && cookies.length > 0){
	for(int i=0; i < cookies.length; i++){
		if(cookies[i].getName().equals("name")){
			Cookie cookie = new Cookie("name",
					URLEncoder.encode("홍길동","UTF-8"));
			response.addCookie(cookie);
		}
	}
}
%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
</body>
</html>
```

#### 쿠키 삭제

Cookie 클래스는 쿠키를 삭제하는 기능을 별도로 제공하고 있지는 않으며, 유효 시간을 0으로 지정해준 후 응답 헤더에 추가해주면, 웹 브라우저가 관련 쿠키를 삭제하게 된다.
`setMaxAge(0)`

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.net.URLEncoder" %>
<%
Cookie[] cookies = request.getCookies();
if (cookies != null && cookies.length > 0){
	for(int i=0; i < cookies.length; i++){
		if(cookies[i].getName().equals("name")){
			Cookie cookie = new Cookie("name","");
			cookie.setMaxAge(0);
			response.addCookie(cookie);
		}
	}
}
%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
</body>
</html>
```

### 쿠키 도메인

- 기본적으로 쿠키는 그 쿠키를 생성한 서버에만 전송
- 도메인 지정 시, 해당 도메인에 쿠키 전달
- `Cookie.setDomain()` 으로 쿠키 설정
- setDomain()의 값으로 현재 서버의 도메인 및 상위 도메인만 전달할 수 있다.
- 도메인 형식
  - .somehost.com - 점으로 시작하는 경우 관련 도메인에 모두 쿠키를 전송한다.
  - www.somehost.com - 특정 도메인에 대해서만 쿠키를 전송한다.
- 웹 브라우저는 도메인이 벗어난 쿠키는 저장하지 않음
- 쿠키 도메인에 따라 쿠키가 전달

### 쿠키 경로

- 경로 미 설정 시, 요청 URL의 경로에 대해서만 쿠키 전달
- 경로 설정 시, 설정한 경로 및 그 하위 경로에 대해서만 쿠키 전달
- Cookie.setPath()로 경로 설정
- 쿠키는 공통으로 사용되는 경우가 많기 때문에 대부분 쿠키의 경로를 "/" 로 지정

### 유효기간

- 유효 시간 미 지정 시 혹음 음수 값일 경우, 웹 브라우저 닫을 때 쿠키도 함께 삭제
- Cookie.setMaxAge()로 쿠키 유효 시간 설정
- 유효 시간이 지나지 않을 경우 웹 브라우저를 닫더라도 쿠키가 삭제되지 않고, 이후 웹 브라우저를 열었을 때 해당 쿠키 전송됨
- 유효 시간 : 초 단위로 설정

### 쿠키 사용 클래스 만들기

```java
import java.io.IOException;
import java.net.URLDecoder;
import java.net.URLEncoder;
import java.util.HashMap;
import java.util.Map;

import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServletRequest;

public class CookieBox {
	private Map<String, Cookie> cookieMap = new HashMap<String, Cookie>();

	public CookieBox(HttpServletRequest request){
		Cookie[] cookies = request.getCookies();
		if(cookies != null ){
			for(int i=0; i < cookies.length; i++){
				cookieMap.put(cookies[i].getName(), cookies[i]);
			}
		}
	}

	public static Cookie createCookie(String name, String value) throws IOException{
		return new Cookie(name, URLEncoder.encode(value, "UTF-8"));
	}

	public static Cookie createCookie(String name, String value, String path, int maxAge) throws IOException{
		Cookie cookie = new Cookie(name, URLEncoder.encode(value, "UTF-8"));
		cookie.setPath(path);
		cookie.setMaxAge(maxAge);
		return cookie;
	}

	public static Cookie createCookie(String name, String value, String domain, String path, int maxAge) throws IOException{
		Cookie cookie = new Cookie(name, URLEncoder.encode(value, "UTF-8"));
		cookie.setDomain(domain);
		cookie.setPath(path);
		cookie.setMaxAge(maxAge);
		return cookie;
	}

	public Cookie getCookie(String name){
		return cookieMap.get(name);
	}

	public String getValue(String name) throws IOException{
		Cookie cookie = cookieMap.get(name);
		if(cookie == null){
			return null;
		}
		return URLDecoder.decode(cookie.getValue(),"UTF-8");
	}

	public boolean exists(String name){
		return cookieMap.get(name) != null;
	}
}
```

## 세션(session)

쿠키가 웹 브라우저에 정보를 보관할 때 사용된다면, 세션은 웹 컨테이너에 정보를 보관할 때 사용된다.
세션은 오직 서버에서만, 그리고 클라이언트마다 생성된다.

### 세션 생성

- page 디렉티브의 session 속성 값을 true로 지정
  - 세션이 존재하지 않을 경우 세션이 생성되고, 세션이 존재할 경우 이미 생성된 세션을 사용
- session 기본 객체를 이용해서 세션에 접근
  - session의 기본 값은 true이므로 false로 하지 않는 이상 항상 세션 사용

### 종료 & 타임아웃

- `session.invalidate()` 을 이용해서 세션 종료
- 세션이 종료되면 기존에 생성된 세션이 삭제
- 이후 접근 시 새로운 세션 생성 됨
- 마지막 세션 사용 이후 유효 기간이 지나면 자동 종료

```xml
//파일명: web.xml
//분 단위 지정
<session-config>
  <session-timeout>50</session-timeout>
</session-config>
```

```java
//초 단위 지정
<%
	session.setMaxInactiveInterval(60*60);
%>
```
