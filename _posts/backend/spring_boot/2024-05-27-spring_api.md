---
title: Spring Boot - API
date: 2024-05-27 19:55:00 +09:00
categories: [Back-End, Spring Boot]
tags: [Back-End, Java, Spring Boot, API]
---

## Ⅰ. GET

### 1. RequestMapping

`RequestMapping`의 경우 별다른 설정을 하지 않으면 모든 종류의 요청을 받는다.
- 아래와 같이 `method` 속성의 값을 `RequestMethod.GET`으로 지정함으로써 `GET` 요청만 받을 수 있다.

```java
@RestController
@RequestMapping("/api/v1/get-api")
public class GetController {

  @RequestMapping(value="/hello", method=RequestMethod.GET)
  public String getHello() {
      return "Hello World";
  }
  
}
```

스프링 4.3 버전 이후로 아래 어노테이션이 사용됨에 따라 `RequestMapping`이 쓰이지 않음
- `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping`

---

### 2. 매개변수가 없는 GET

매개변수가 없는 요청은 설정된 url 그대로 입력
- 스프링부트는 정해진 응답 그대로 반환

```java
@GetMapping("/name")
public String getName() {
    return "Koneweekk";
}
```
---

### 3. @PathVariable

실무환경에선 거의 대부분 매개변수를 받는 메서드를 작성하게됨
- 그 중 많이 쓰이는 방법 중 하나가 url 자체에 값을 담아 요청

```java
@GetMapping("/name/{variable}")
public String getMethodName(@PathVariable String variable) {
    return name;
}
```

- `@GetMapping`의 url 값에서 변수로 쓰일 부분에 `{}`를 입력
- `@PathVariable`를 메서드의 매개변수에 붙여 url 매개변수임을 선언
- 이 때 두 변수의 이름은 같아야함

```java
@GetMapping("/name/{variable}")
public String getMethodName(@PathVariable("variable") String name) {
    return name;
}
```

- 변수명을 같게 하기 어렵다면 `@PathVariable`의 괄호를 열어 두 변수명을 매칭

---

### 4. @

url 쿼리 스트링을 통해 매개변수를 전달하는 방법도 존재
- `url?key1=val1&key2=val2` 형식

```java
@GetMapping("/request?name=val1&email=val2")
public String getRequestParam1(@RequestParam String name, @RequestParam String email) {
    return name + email;
}
```

- `@GetMapping`의 url뒤 `?`를 붙이고 `key=value` 형태로 입력
- 