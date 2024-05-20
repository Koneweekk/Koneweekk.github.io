---
title: Spring Boot - MVC 패턴 이해와 실습
date: 2024-05-20 16:20:00 +09:00
categories: [Back-End, Spring Boot]
tags: [Back-End, Java, Spring Boot, MVC, Mustache]
---

## Ⅰ. 뷰 템플릿과 MVC 패턴

### 1. 뷰 템플릿이란?

> 웹 애플리케이션에서 사용자에게 보여지는 UI를 정의하는 파일

- 보통 HTML과 같은 마크업 언어와 함께 템플릿 언어를 사용하여 동적으로 콘텐츠를 생성할 수 있게 한다.

---

### 2. MVC 패턴이란?

> 소프트웨어 디자인 패턴으로, 애플리케이션을 세 가지 주요 컴포넌트로 나누어 구조화

Model (모델)
- 데이터와 그 데이터를 처리하는 비즈니스 로직을 담당
- 데이터베이스와의 상호작용, 상태 관리 등을 포함
  
View (뷰)
- 사용자에게 보여지는 UI를 담당
- 델의 데이터를 기반으로 화면에 렌더링

Controller (컨트롤러)
- 사용자의 입력을 처리하고, 이를 모델과 뷰로 전달
- 모델의 상태를 변경하거나, 뷰를 업데이트하는 역할

---
<br>

## Ⅱ. 뷰 템플릿 페이지 만들기

### 1. 뷰 템플릿 페이지

뷰 템플릿은 `templates` 디렉터리 안에 만든다.


```html
<!-- src / main / resources / templates / grettings.mustache -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <h1>안녕하세요!</h1>
</body>
</html>
```

---

### 2. 컨트롤러

컨트롤러는 기본 패키지에 controller라는 패키지를 만들어 추가한다.

```java
// src / main / java / kr.or.kosa.springproject1.controller
package kr.or.kosa.springproject1.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;


@Controller
public class FirstController {
  
  @GetMapping("/hi")
  public String meetYou(Model model) {
    return "greeting";
  }
}
```

`/hi`라는 주소를 입력하면 서버가 `greeting.mustache` 을 보내게 된다.

---

### 3. 모델

```java
package kr.or.kosa.springproject1.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;


@Controller
public class FirstController {
  
  @GetMapping("/hi")
  public String meetYou(Model model) {
    model.addAttribute("username", "hanju");
    return "greeting";
  }
}
```

- `greeting.mustache` 문서에 `username` 변수에 `hanju`값을 담아 보내게 된다.

```html
<!-- src / main / resources / templates / grettings.mustache -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <h1>{{username}}님 안녕하세요!</h1>
</body>
</html>
```

- 서버가 넘기는 매개변수에 따라 출력값이 변하게 된다.
- 위 경우 `hanju님 안녕하세요!` 라는 출력값이 뜨게 된다.