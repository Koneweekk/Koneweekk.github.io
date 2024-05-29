---
title: Spring Boot - 테스트 코드
date: 2024-05-29 12:00:00 +09:00
categories: [Back-End, Spring Boot]
tags: [Back-End, Java, Spring Boot, Test Code]
---

## Ⅰ. 테스트 코드 개요

### 1. 테스트 코드란?

> 말그대로 우리가 작성한 코드 혹은 로직 자체를 테스트하기 위한 코드

- 애플리케이션의 다양한 부분을 다양한 방식으로 테스트
- 버그를 조기에 발견하고 코드의 품질을 유지하는 데 중요한 역할

---

### 2. 테스트 코드를 작성하는 이유

#### 개발 과정에서 문제를 미리 발견
- 코드에 잠재된 문제를 발견하는데 큰 도움
- 문제가 발생할 수 있는 여러 상황에 맞춰 테스트 코드를 작성해야함

#### 리팩토링 리스크 감소
- 일반적으로 애플리케이션 개발 후 서비스 업데이트를 위해 계속해서 코드 추가 및 수정
- 이러한 과정은 연관된 코드에 영향을 주는 작업
- 테스트 코드가 작성되어 있다면 테스트 코드 실행만으로 부작용에 대비할 수 있음

#### 명세 문서로의 기능 수행
- 협업시 타인의 코드 이해와 충돌 방지를 위한 명세로서 유용한 기능
- 동작 검증을 위한 테스트 코드를 애플리케이션 코드와 비교해보며 작성자의 의도 파악 용이
- 애플리케이션을 직접 테스트하는 것보다 빠르게 진행

---

### 2. 단위 테스트와 통합 테스트

#### 단위 테스트

> 애플리케이션의 개별 모듈을 독립적으로 확인

- 가장 작은 단위의 테스트 방식
- 일반적으로 메서드 단위로 테스트 수행
- 메서드 호출이 의도한 결과값을 반환하는지 확인
- 비용이 적게 들기 때문에 테스트 피드백을 빠르게 받음

#### 통합 테스트

> 애플리케이션을 구성하는 다양한 모듈을 결합해 전체 로직이 의도한 대로 작동하는지 확인

- 모듈을 통합하는 과정에서 호환성을 포함해 애플리케이션의 정상적인 동작을 확인
- 여러 모듈을 함께 테스트해 정상적인 로직 수행이 가능한지 확인
- 단위 테스트에 비해 데이터베이스나 네트워크 같은 외부 요인들도 포함
- 테스트 비용이 크다는 단점이 존재

#### ※ 테스트 비용

- 금전적인 비용, 시간, 인력 등 개발에 필요한 모든 것을 포괄
- 통계적으로 개발 과정에 60%, 테스트 과정에 40%의 비용이 서비스 개발 시 발생하는 것을 알려짐

---

### 3. Given-When-Then 패턴

- 테스트 주도 개발에서 파생된 BDD(Behavior-Driven-Development)를 통해 탄생한 테스트 접근 방식
- 일반적으로 인수 테스트(비교적 많은 환경을 포함해 테스트)에 적합
- 불필요하게 코드가 길어지기 때문에 단위 테스트에선 잘 사용되지 않음

#### Given
- 테스트를 수행하기 전에 테스트에 필요한 환경을 설정하는 단계
- 테스트에 필요한 변수를 정의하거나 Mock 객체를 통해 특정 상황에 대한 행동을 정의

#### When
- 테스트 목정르 보여주는 단계
- 실제 테스트 코드가 포함되며, 테스트를 통한 결괏값을 가져오게 됨

#### Then
- 테스트의 결과를 검증하는 단계
- 일반적으로 When 단계에서 나온 결괏값 검증을 수행

---

### 4. F.I.R.S.T

> 좋은 테스트 코드의 5가지 규칙

#### Fast
- 테스트는 빠르게 수행되어야함

#### Isolated
- 하나의 테스트 코드는 목적 대상에 대해서만 수행되어야함

#### Repeatable
- 어떤 환경에서도 반복 가능해야함

#### Self-Validating
- 그 자체만으로도 테스트의 검증이 완료되어야함
- 테스트의 성공 실패 여부확인할 수 있는 코드가 동반되어야함

#### Timely
- 테스트하려는 애플리케이션 코드 구현 전에 완성되어야함

---
<br>

## Ⅱ. JUnit을 활용한 테스트 코드 작성

### 1. JUnit이란?

> 자바 언어에서 사용되는 대표적인 테스트 프레임워크

- 단위테스트 뿐만 아니라 통합 테스트 기능도 제공
- 어노테이션 기반 테스트 방식을 지원함으로써 간편한 사용 가능
- 단정문(assert)를 통해 테스트 케이스의 기대값이 정상적으로 도출됐는지 확인 가능

---

### 2. JUnit의 세부 모듈

> 크게 Jupiter, Platform, Vintage 로 구성

![JUnit 세부 모듈](https://static.packt-cdn.com/products/9781787285736/graphics/93231e34-6b81-4e81-9d94-e83e7742885e.png)

#### JUnit Platform
- JVM에서 테스트를 시작하기 위한 뼈대 역할
- 테스트 엔진 : 테스트를 발견, 테스트 수행, 그 결과를 보고
- 각종 IDE와의 연동을 보조
- TestEngine API, Console Launcehr, JUnit 4 Based Runner등이 포함

#### JUnit Jupiter
- JUnit 5에 대한 테스트 엔진 API의 구현체를 포함
- Jupiter Engine : Jupiter API를 활용해서 작성한 테스트 코드를 발견하고 실행하는 역할 수행
  
#### JUnit Vintage
- JUnit 3, 4에 대한 테스트 엔진 API를 구현

---

### 3. JUnit 설정

build.gradle에 의존성이 잘 주입되었는지 확인

```
dependencies {
  ...
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
  ...
}
```
`spring-boot-starter-test`가 제공하는 대표적인 라이브러리는 다음과 같다.

- JUnit 5 (Jupiter): 단위 테스트를 작성하고 실행하는 데 사용되는 주요 테스트 프레임워크
- Spring Test & Spring Boot Test: 스프링과 스프링 부트를 테스트하기 위한 도구들을 제공
- AssertJ: 다양한 단정문(assertion) 기능을 제공하는 라이브러리
- Hamcrest: 매처 기반의 단언을 작성할 수 있는 라이브러리
- Mockito: 모킹 프레임워크로, 의존성을 가짜(mock)로 만들어 단위 테스트를 수행
- JSONassert: JSON 데이터를 검증할 수 있는 라이브러리
- JsonPath: JSON 데이터를 탐색하고 추출할 수 있는 라이브러리

---

### 4. JUnit 생명 주기

생명주기와 관련되어 테스트 순서에 관여하게 되는 대표적인 어노테이션들은 다음과 같다.

- `@BeforeAll` : 모든 테스트 메서드가 실행되기 전에 한 번 실행(반드시 static 메서드)
- `@AfterAll` : 모든 테스트 메서드가 실행된 후에 한 번 실행(반드시 static 메서드)
- `@BeforeEach` : 각 테스트 메서드가 실행되기 전에 실행
- `@AfterEach` : 각 테스트 메서드가 실행된 후에 실행
- `@Test` : 테스트 메서드를 표시. 이 메서드는 테스트 프레임워크에 의해 실행

```java
package com.springboot.test;

import org.junit.jupiter.api.AfterAll;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeAll;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Disabled;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

public class TestLifeCycle {

  @BeforeAll
  static void beforAll() {
    System.out.println("## BeforeAll Anotation 호출 ##");
    System.out.println();
  }
  
  @AfterAll
  static void afterAll() {
    System.out.println("## AfterAll Anotation 호출 ##");
    System.out.println();
  }

  @BeforeEach
  void beforeEach() {
    System.out.println("## BeforeEach Anotation 호출 ##");
    System.out.println();
  }

  @AfterEach
  void afterEach() {
    System.out.println("## AfterEach Anotation 호출 ##");
    System.out.println();
  }

  @Test

  void test1() {
    System.out.println("test1 시작");
    System.out.println();
  }

  @Test
  @DisplayName("test2입니다")
  void test2() {
    System.out.println("test2 시작");
    System.out.println();
  }

  @Test
  @Disabled
  void test3() {
    System.out.println("test3 시작");
    System.out.println();
  }
}
```

위 코드를 test 폴더 안에 작성하고 실행하면 결과는 다음과 같다.

```
## BeforeEach Anotation 호출 ##

test1 시작

## AfterEach Anotation 호출 ##

## BeforeEach Anotation 호출 ##

test2 시작

## AfterEach Anotation 호출 ##

## AfterAll Anotation 호출 ##
```

- `@Disabled`를 선언한 `test3` 메서드는 실행되지 않은 것을 볼 수 있다.

---

### 5. 컨트롤러 객체 테스트


