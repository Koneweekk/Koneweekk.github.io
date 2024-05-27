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

### 4. @RequestParam

url 쿼리 스트링을 통해 매개변수를 전달하는 방법도 존재
- `url?key1=val1&key2=val2` 형식

```java
@GetMapping("/request?name=val1&email=val2")
public String getRequestParam1(@RequestParam String name, @RequestParam String email) {
    return name + email;
}
```

- `@GetMapping`의 url뒤 `?`를 붙이고 `key=value` 형태 쿼리스트링을 입력
- 메서드의 매개변수에 `@RequestParam`을 붙이고 key의 이름과 동일한 변수명 선언

```java
@GetMapping("/request2")
    public String getRequestParam2(@RequestParam Map<String,String> param) {

    StringBuilder sb=  new StringBuilder();

    param.entrySet().forEach(map -> {
        sb.append(map.getKey() + " : " + map.getValue() + "\n");
    });

    return sb.toString();
}
```

- 만약 쿼리스트링에 어떤 값이 들어올지 모른다면 `Map` 객체를 활용할 수도 있음
- 이런 형태의 코드는 값에 상관없이 요청을 받을 수 있음

---

### 5. DTO 객체 활용

> DTO(Data Transfer Object) : 데이터 전송 객체로, 주로 계층 간 데이터 전송을 목적으로 사용

```java
package com.springboot.dto;

import lombok.AllArgsConstructor;
import lombok.Getter;
import lombok.Setter;
import lombok.ToString;

@Getter
@Setter
@ToString
@AllArgsConstructor
public class MemberDto {
  private String name;
  private String email;
}
```

- `lombok`을 통해 간단하게 맴버 정보에 관한 DTO 클래스를 생성 

```java
@GetMapping("/request3")
public String getRequestParam3(MemberDto memberDto) {
    return memberDto.toString();
}
```

- 위에서 생성한 DTO로 쿼리스트링 형태로 요청 데이터를 전달받음
- 위의 `@RequestParam`을 활용한 방식에서 코드량을 줄일 수 있음

---

### ※ DTO와 VO

#### DTO (Data Transfer Object)

- 데이터 전송 객체로 주로 계층 간 데이터 전송을 목적으로 사용됨
- 속성만을 가짐: 비즈니스 로직이 없고, 데이터의 속성만을 포함
- 직렬화 가능: 데이터를 직렬화하여 네트워크를 통해 전송하거나, 파일에 저장할 수 있음
- 단순성: 데이터 전송을 위한 단순한 구조로, 종종 getter와 setter 메서드만을 가짐

#### VO (Value Object)

- 값 객체로, 주로 특정 데이터를 표현하고, 그 데이터가 불변(immutable)임을 보장하는 데 사용
- VO는 데이터의 값 자체를 나타내며, 동일한 값을 가지면 동일한 객체로 간주
- 불변성: 객체 생성 후 속성값이 변경되지 않음
- 동등성 비교: 객체의 동일성은 참조가 아닌 값으로 비교됨
- 비즈니스 의미: 특정 비즈니스 의미를 가지며, 관련된 메서드를 포함할 수 있음

---
<br>

## Ⅱ. POST

### 1. POST API의 특징

> 웹 어플리케이션 서버를 통해 데이터베이스 등 저장소에 데이터를 저장할 때 사용

- GET요청이 URL이나 파라미터에 변수를 넣는 반면 POST 요청은 HTTP 바디에 데이터를 담아서 전송
- 데이터를 보내는 형식은 일반적으로 JSON 형식

---

### 2. @RequestBody

- HTTP 바디 내용을 해당 어노테이션이 지정된 객체에 매핑하는 역할
- `@RestController`로 선언된 클래스 내부에선 생략 가능
- DTO 형태 데이터를 자동으로 JSON 형태로 전달

```java
@RestController
@RequestMapping("/api/v1/post-api")
public class PostController {

    @PostMapping("/member")
    public String postMember(@RequestBody Map<String, Object> param) {
        StringBuilder sb=  new StringBuilder();

        param.entrySet().forEach(map -> {
        sb.append(map.getKey() + " : " + map.getValue() + "\n");
        });

        return sb.toString();
    }

}
```

- 요청값을 특정할 수 없다면 `Map` 형태로 데이터를 전달하는 것이 편함


```java
@PostMapping("/member2")
public String postMember2(@RequestBody MemberDto memberDto) {
    return memberDto.toString();
}
```

- 요청값을 특정할 수 있다면 DTO 객체를 통해 매개변수를 입력받을 수 있음

---

### ※ JSON(Javascript Object Notation)

- 자바스크립트의 객체 문법을 따라는 문자 기반의 데이터 포맷
- 경량성: JSON은 간단한 텍스트 형식으로 데이터 구조를 표현하기 때문에 경량적임
- 가독성: 사람이 읽고 쓰기 쉬운 구조를 가지고 있음
- 호환성: 대부분의 프로그래밍 언어에서 JSON을 쉽게 처리할 수 있는 라이브러리를 제공
- 유연성: 다양한 데이터 구조를 표현할 수 있음

---
<br>

## Ⅲ. PUT

### 1. PUT API의 특징

> 웹 애플리케이션 서버를 통해 저장소의 데이터 값을 업데이트하는데 사용

- 컨트롤러 클래스 구현 방법은 POST API와 거의 동일
- 즉, HTTP 바디를 통해 데이터를 전달

---

### 2. @RequestBody

```java
@RestController
@RequestMapping("/api/v1/put-api")
public class PutController {
  
  @PutMapping("/member")
  public String putMember(@RequestBody Map<String, Object> param) {
    StringBuilder sb=  new StringBuilder();

    param.entrySet().forEach(map -> {
      sb.append(map.getKey() + " : " + map.getValue() + "\n");
    });

    return sb.toString();
  }

  @PutMapping("/member2")
  public String putMember2(MemberDto memberDto) {
      return memberDto.toString();
  }

}
```

- PUT 요청도 POST와 동일하게 값이 특정되면 Map 형식을 그 외엔 DTO 형식으로 데이터 전송 가능

---

### 3. ResponseEntity

#### HttpEntity

- 스프링 프레임워크에서 지원하는 클래스
- Header와 Body로 구성된 HTTP 요청과 응답을 구성하는 역할
- `RequestBody`와 `ResponseEntity`가 `HttpEntity`를 상속받은 구현 클래스

#### ResponseEntity
- 서버에 들어온 요청에 대해 응답 데이터를 구성해서 전달할 수 있음
- `HttpEntity`로부터 `HttpHeaders`와 `Body`를 가지고 자체적으로 `HttpStatus`를 구성
- 이 클래스를 활용하여 응답 코드 변경, Header와 Body의 구성을 더욱 쉽게 할 수 있음
- PUT말고도 모든 메서드에 활용 가능

```java
@PutMapping("/member3")
public ResponseEntity<MemberDto> putMember3(@RequestBody MemberDto memberDto) {
return ResponseEntity
        .status(HttpStatus.ACCEPTED)
        .body(memberDto);
}
```

- 메서드의 리턴타입을 `ResponseEntity`로 설정하고 다양한 응답 코드를 함께 전달

---

## Ⅳ. DELETE

### 1. DELETE API의 특징

> 웹 어플리케이션 서버를 통해 저장소의 데이터를 삭제할 때 사용

- 서버에서는 클라이언트로부터 데이터를 식별할 수 있는 값을 받아 데이터를 삭제
- 이 때 보통 컨트롤러는 간단한 값을 입력받음

---

### 2. @PathVariable & @RequestParam

```java
@RestController
@RequestMapping("/api/v1/delete-api")
public class DeleteController {
  
  @DeleteMapping(value="/{variable}")
  public String deleteMember(@PathVariable String variable) {
    return variable;
  }

  @DeleteMapping(value="/delete")
  public String deleteMember2(@RequestParam String email) {
    return email;
  }

}
```

- Get요청과 마찬가지로 `@PathVariable`과 `@RequestParam`을 통해 간단히 값을 입력받을 수 있음

---
<br>

## Ⅴ. Swagger

### 1. API 명세화

> API의 구조, 기능, 사용 방법을 명확하게 정의하고 문서화하는 과정

- 개발자 간의 오해를 줄이고, 명확한 커뮤니케이션을 가능케함
- API가 업데이트되거나 변경될 때, 명세서를 참조하여 변경 사항을 쉽게 파악하고 반영할 수 있음

API가 개발 과정에서 주기적으로 변경됨에 따라 명세 문서 또한 같이 변경되어야한다.
- 이러한 문제를 해결해주는 것이 Swagger라는 오픈소스 프로젝트

---

### 2. 설정

우선 build.gradledp 의존성을 주입해준다.
- spring boot 3.xx 버전부터 기존 SpringFox가 호환되지 않는 문제 발생
- 해결하기 위해 아래처럼 의존성 주입이 필요

```
dependencies {
    ...
    implementation 'org.springdoc:springdoc-openapi-starter-webmvc-ui:2.0.2'
	implementation group: 'io.springfox', name: 'springfox-swagger2', version: '2.9.2'
	implementation group: 'io.springfox', name: 'springfox-swagger-ui', version: '2.9.2'
    ...
}
```

이후 config란 패키지 안에 설정 코드 관련 클래스를 만들어 준다

```java
@Configuration
public class SwaggerConfiguration {
  @Bean
  public OpenAPI openAPI() {
      return new OpenAPI()
              .components(new Components())
              .info(apiInfo());
  }

  private Info apiInfo() {
      return new Info()
              .title("Spring Boot REST API Specifications")
              .description("Specification")
              .version("1.0.0");
  }
}
```

---

### 3. Swagger의 활용



<img src="/assets/img/post/backend/spring-boot/2024-05-27-spring_api/01.png" width="100%" title="01" alt="01"/> 

주소창에 `루트주소/swagger-ui/index.html`를 입력하면 위와 같이 swagger ui에 접근 가능

```java
@ApiOperation(value = "GET 메서드 예시", notes = "@RequestParam을 활용")
@GetMapping("/request")
public String getRequestParam1(
@ApiParam(value = "이름", required = true) @RequestParam String name,
@ApiParam(value = "이메일", required = true) @RequestParam String email
) {
return name + " : " + email;
}
```

위와 같은 방식으로 상세하게 Swagger의 명세를 입력 가능하다.
- `@ApiOperation` : 대상 API의 설명을 작성
- `@ApiParam` : 매개변수에 대한 설명 및 설정. DTO 클래스 내의 매개변수도 정의 가능

<img src="/assets/img/post/backend/spring-boot/2024-05-27-spring_api/02.png" width="100%" title="02" alt="02"/> 

결과는 위와 같다.