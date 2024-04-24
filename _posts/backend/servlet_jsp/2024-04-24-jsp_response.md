---
title: JSP - response 객체
date: 2024-04-24 10:35:00 +09:00
categories: [Back-End, Servelt & JSP]
tags: [Back-End, Servelt, JSP, response]
---

## Ⅰ. response 객체 개요

### 1. response 객체란?

> 클라이언트에게 응답을 보내는 데 사용

클라이언트로 데이터를 전송하거나 응답 관련 설정을 하는 역할을 한다.

---

### 2. 표현식(출력)

- `<%= %>` 구문을 사용하면 JSP 페이지에서 `response` 객체를 직접 사용하지 않음
- 하지만 결과를 출력하는 과정에서 내부적으로 `response` 객체가 관여하게 됩니다.

```jsp
<body>
		1. 일반적인 출력 담당 response : <%=100+200+300%> <br>
</body>
```

---
<br>

## Ⅱ. 관련 메서드

### 1. sendRedirect

클라이언트를 다른 URL로 리다이렉션할 수 있다.

```jsp
<%
  response.sendRedirect("other_page.jsp");
%>
```

자바스크립트의 `location.href`와도 같은 역할을 한다.

```js
location.href = "other_page.jsp"
```