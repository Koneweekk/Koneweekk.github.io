---
title: Spring Boot - Spring Boot란?
date: 2024-05-27 16:30:00 +09:00
categories: [Back-End, Spring Boot]
tags: [Back-End, Java, Spring Boot]
---

## Ⅰ. Spring 부트란?

### 1. 스프링 프레임워크

> 자바에서 가장 많이 사용하는 경량 웹 프레임워크

[스프링 프레임워크란?](/posts/spring_setting/)

---

### 2. 스프링 레거시 vs 스프링 부트

#### 의존성 관리

- 스프링부트에선 `spring-boot-starter`라는 의존성을 제공
- 각 라이브러리의 기능과 관련해서 자주 사용되고 화환되는 버전의 모듈 조합을 제공

#### 자동 설정

- 스프링을 사용하기 위한 자동 설정을 지원
- 애플리케이션을 개발하는데 필요한 의존성을 추가하면 프레임워크가 자동으로 관리

#### 내장 WAS

- 스프링 부트는 내장 WAS(톰캣) 존재
- 스프링 부트의 자동 설정 기능은 톰캣에도 적용되어 특별한 설정없이 톰캣 실행 가능

#### 모니터링

- 스프링부트 액추에이터(Spring Boot Actuator)라는 자체 모니터링 도구 내장

---
<br>

## Ⅱ. 스프링부트 애플리케이션 개발

### 1. 프로젝트 생성하기

스프링 공식 사이트에서 프로젝트 생성 가능
- 각 항목을 선택 후 Generate

<img src="/assets/img/post/backend/spring-boot/2024-05-27-spring_start/01.png" width="100%" title="01" alt="01"/>

ADD Dependencies를 클릭하여 필요한 의존성을 주입
<img src="/assets/img/post/backend/spring-boot/2024-05-27-spring_start/02.png" width="100%" title="02" alt="02"/> 

---

### 2. gradle

> 소프트웨어 프로젝트의 자동화 시스템으로, 주로 빌드, 테스트, 배포, 그리고 더 복잡한 워크플로우를 관리하는 데 사용

성능 최적화를 위해 여러 가지 기능을 제공
- 인크리멘탈 빌드: 변경된 부분만 다시 빌드하여 시간과 리소스를 절약합니다.
- 빌드 캐싱: 이전 빌드의 결과를 재사용하여 빌드 속도를 크게 향상시킵니다.

유연한 의존성 관리
- Maven 중앙 저장소, JCenter, 그리고 자신만의 맞춤형 저장소를 포함한 다양한 소스에서 의존성을 관리
 이를 통해 프로젝트의 라이브러리와 플러그인을 쉽게 추가하고 관리 가능

아래는 프로젝트 빌드 정보를 담은 build.gradle 파일이다.

```gradle
plugins {
	id 'java'
	id 'org.springframework.boot' version '3.2.6'
	id 'io.spring.dependency-management' version '1.1.5'
}

group = 'com.springboot'
version = '0.0.1-SNAPSHOT'

java {
	sourceCompatibility = '17'
}

configurations {
	compileOnly {
		extendsFrom annotationProcessor
	}
}

repositories {
	mavenCentral()
}

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-web'
	compileOnly 'org.projectlombok:lombok'
	annotationProcessor 'org.springframework.boot:spring-boot-configuration-processor'
	annotationProcessor 'org.projectlombok:lombok'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
	testRuntimeOnly 'org.junit.platform:junit-platform-launcher'
}

tasks.named('test') {
	useJUnitPlatform()
}
```

---

### 3. 컨트롤러 작성

> 웹 애플리케이션에서 HTTP 요청을 처리하고, 클라이언트에게 적절한 응답을 반환하는 역할

특별한 경우를 제외한 모든 요청은 controller를 통해야한다.

<img src="/assets/img/post/backend/spring-boot/2024-05-27-spring_start/03.png" width="100%" title="03" alt="03"/>
- controller란 패키지를 생성 후, HelloController.java 파일 생성

```java
package com.springboot.hello.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {
  
  @RequestMapping("/hello")
  public String hello() {
    return "Hello World";
  }
  
}
```

---

### 4. 프로젝트 실행

프로젝트가 성공적으로 실행되면 아래와 같은 로그가 찍히는 것을 확인 가능

<img src="/assets/img/post/backend/spring-boot/2024-05-27-spring_start/04.png" width="100%" title="04" alt="04"/>

http://localhost:{설정한 포트 번호}/hello를 주소창에 입력하면 Hello World가 잘 찍히는 것을 볼 수 있다.

<img src="/assets/img/post/backend/spring-boot/2024-05-27-spring_start/05.png" width="100%" title="05" alt="05"/>

포트 번호는 application.properties 파일에 다음과 같이 추가한다.

```
server.port=8090
```