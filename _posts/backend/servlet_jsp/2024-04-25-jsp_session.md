---
title: JSP - session 객체
date: 2024-04-25 10:45:00 +09:00
categories: [Back-End, Servelt & JSP]
tags: [Back-End, Servelt, JSP, cookie, session]
---

## 세션(Session)

### 1. 세션 객체란?

> 서버(Web)에 접속한 사용자 마다 고유하게 부여되는 객체 

- 모든 페이지에서 공유 가능한 객체
- 고유성 보장 : 세션ID를 클라이언트(session 객체)마다 고유하게 부여 
- 사용자와 관련된 정보 저장 : 로그인ID , 장바구니, 사용자의 정보, 접속한 시간, 마지막 접속 시간 등등

---

### 2. 기본 속성

- `getId()` : 세션 ID를 반환
- `getCreationTime()` : 세션 생성 시간 반환
- `getLastAccessedTime()` : 마지막 접속 시간 반환

```jsp
<%
Date time = new Date();
SimpleDateFormat formatter = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
%>

<h3>session객체 정보</h3>
session 객체의 식별값 : <%=session.getId()%>
<hr>

<%
time.setTime(session.getCreationTime());
%>

[session 이 생성된 시간 ] : <%=formatter.format(time)%>
<hr>

<%
time.setTime(session.getLastAccessedTime());
%>

[session 마지막 접속 시간 :client ] : <%=formatter.format(time)%>
```

---

### 3. 사용자 속성 정의

- `setAttribute()` : 세션의 속성을 생성하고 값 정의
- `getAttribute()` : 관련 속성값 반환

```jsp
<h3>세션정보</h3>
sessionID : <%= session.getId() %>
<hr>
<%
  String userid = request.getParameter("userid");
  session.setAttribute("id", userid); //서버에 저장
%>

<h3>내가 저장한 session 이 필요하면</h3>
<%
  String id = (String)session.getAttribute("id");
  out.print("당신의 ID는" + id + "입니다 ^^");
%>
```

---

### 4. 세션 소멸

- `invalidate()` : 현재 세션을 소멸시킴

```jsp
<%
	//로그아웃
	session.invalidate();
	out.print("<script>location.href='login.jsp'</script>");
%>
```

---

### 5. application 객체와의 차이점

application 객체
- 모든 페이지에서 공유 가능한 전역 변수
- 모든 session (접속한 모든 사용자) 공유
- 서버 리부팅 혹은 Tomcat 재실행시 소멸

session 객체
- 모든 페이지에서 공유 가능한 전역 변수
- 한 사용자만 사용 가능
- 서버 리부팅 , Tomcat 재실행 , `session.invaildate()` 실행시 소멸

---
<br>

## ※ 쿠키(Cookie)

### 1. 쿠키 생성

쿠키 객체를 통하여 쿠키 생성 가능
- 서버에서 쿠키 생성
- 클라이언트 브라우져에게 전달하지 않음

```java
Cookie mycookie = new Cookie("cname","1004");
response.addCookie(mycookie);
```

소멸 시간을 설정할 수 있음
- 이 때 단위는 초단위

```java
Cookie co = new Cookie("kosa","hong");
// 한 달 분량의 쿠키 설정
co.setMaxAge(30*24*60*60);
response.addCookie(co);
```

---

### 2. 쿠키 읽기

`request` 객체의 메서드를 통해 쿠키를 읽어올 수 있음
- 이 때 모든 쿠키가 배열형태로 반환됨
- `getName` : 쿠키 이름 반환
- `getValue` : 쿠키 값 반환
- `getMaxAge` : 쿠키 소멸 시간 반환(없으면 -1)
- `getDomain` : 쿠키 도메인 반환

```java
Cookie[] ck = request.getCookies();

if (ck != null || ck.length > 0) {
  for(Cookie c : ck) {
    out.print(c.getName() + "<br>");
    out.print(c.getValue() + "<br>");
    out.print(c.getMaxAge() + "<br>");
    out.print(c.getDomain() + "<br>");
    out.print("<br>");
  }
}
```