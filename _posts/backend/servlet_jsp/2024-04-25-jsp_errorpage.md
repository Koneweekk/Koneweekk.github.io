---
title: JSP - 에러 페이지
date: 2024-04-25 09:50:00 +09:00
categories: [Back-End, Servelt & JSP]
tags: [Back-End, Servelt, JSP, error page, error code]
---

## 1. 글로벌 예외 페이지

web.xml 파일에 네트워크 에러 발생시 띄워줄 에러 페이지를 지정할 수 있다.
- 보통 네트워크 에러코드로 지정
- 모든 페이지에 적용됨

`NullPointException`과 같은 예외별로도 에러페이지를 구성할 수 있음
- 잘 쓰이지 않는 방식

```xml
<error-page>
  <error-code>500</error-code>
  <location>/error/e_500.jsp</location>
</error-page>
<error-page>
  <error-code>404</error-code>
  <location>/error/e_404.jsp</location>
</error-page>
```

---

## 2. 로컬 예외 페이지

특정 페이지에만 적용되는 에러 페이지를 지정할 수 있다.

```jsp
<%@ page errorPage="/error/local.jsp" %>
```