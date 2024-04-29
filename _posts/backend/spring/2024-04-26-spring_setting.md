---
title: Spring - 스프링 개요
date: 2024-04-26 14:05:00 +09:00
categories: [Back-End, Spring]
tags: [Back-End, Java, Spring,]
---

## Ⅰ. 스프링(Spring)이란

### 1. 스프링이란?

> Java 플랫폼을 위한 오픈 소스 웹애플리케이션 프레임워크

- IoC와 AOP를 지원하는 경량의 컨테이너 프레임워크

---

### 2. 스프링의 특징

제어 반전(Inversion of Control, IoC)
- 소프트웨어 개발에서 객체 생성 및 의존성 관리를 프레임워크나 컨테이너에게 맡기는 디자인 원칙
- 기존의 객체 생성 및 의존성 주입 방식은 개발자가 직접 객체를 생성하고 의존성을 관리하는 방식
- 스프링은 Spring Bean 묘듈에서 객채를 생성하고 제어하면 관리

의존성 주입(Dependency Injection, DI)
- 의존성 주입을 통해 객체 간의 결합도를 줄입니다
- 코드의 유연성과 재사용성을 높여주며 유지 보수를 용이하게 합니다.

관점 지향 프로그래밍(Aspect-Oriented Programming, AOP)
- AOP를 지원하여 핵심 비즈니스 로직과 횡단 관심사(Cross-cutting Concerns)를 분리
- 코드의 모듈화와 가독성을 향상시킵니다.
  
트랜잭션 관리
- Spring은 선언적인 방식으로 트랜잭션 관리를 제공하여 데이터베이스 트랜잭션을 쉽게 처리
- 이는 데이터베이스와의 상호작용에서 일관성과 안전성을 유지하는 데 도움
  
테스트 지향 프로그래밍(Test-Driven Development, TDD)
- TDD를 지원하여 단위 테스트, 통합 테스트 및 기능 테스트를 쉽게 작성
- 또한 테스트용 환경을 구성하는 것이 간단하며, 테스트 가능한 코드를 작성하는 데 도움

---

### 3. 스프링 버전 선택

- Spring 3.2x : Java 5 지원
- Spring 4.x : Java 6, 7, 8 지원
- Spring 4.3 : Java 8 지원
- Spring 5.0 : Java 8, 11, 17, 19 지원
- Spring 6.0 : Java 17, 19, 21 지원

지원하는 Java 버전에 맞추어 적절한 스프링 버전을 선택

---
<br>

## Ⅱ. 스프링의 구조

![스프링의 구조](https://imagedelivery.net/v7-TZByhOiJbNM9RaUdzSA/4064615f-ec88-481a-5d96-fcb78740b700/public)

### 1. Core Container

- 스프링의 핵심 기능을 담당하는 모듈
- IoC (Inversion of Control) 컨테이너를 제공합니다
- IoC 컨테이너는 객체의 생성, 관리, 의존성 주입을 담당

Spring Core (스프링 코어)
- IoC (Inversion of Control) 컨테이너와 의존성 주입(Dependency Injection)을 제공
- 빈 (Bean)의 생명주기 관리, 이벤트 발행/구독, 자바 애플리케이션을 위한 기본적인 설정 등을 담당
- 스프링의 기본적인 기능을 포함하고 있으며, 스프링의 다른 모듈들은 이러한 Core에 의존하여 작동

Spring Bean (스프링 빈)
- 빈 : 스프링에서 관리되는 객체, 빈 모듈에 의해 생명주기가 관리됨
- Bean Factory를 통해 bean 인스턴스를 생성하거나 bean의 의존성 문제를 해결

Spring Context (스프링 컨텍스트)
- 스프링 애플리케이션의 설정 정보를 가지고 있으며, 빈의 의존성 주입을 담당
- 스프링 컨텍스트는 스프링 빈을 검색하고 조회할 수 있는 기능을 제공합니다.

SpEL (Spring Expression Language, 스프링 표현 언어)
- 스프링에서 제공하는 표현 언어로, XML 파일이나 어노테이션에서 문자열로 작성하여 사용
- 주로 빈의 프로퍼티 값을 설정할 때, 빈의 조건을 지정할 때, 메소드 호출 시 인수를 전달할 때 사용

---

### 2. Data Access/Integration

- 데이터 액세스와 통합을 위한 모듈
- 데이터베이스와의 상호 작용을 단순화

JDBC (Java Database Connectivity)
- 자바 애플리케이션과 데이터베이스 간의 연결을 제공하는 자바 API
- 데이터베이스에 접근하고 SQL 쿼리를 실행할 수 있도록 지원

ORM (Object-Relational Mapping):
- 객체와 관계형 데이터베이스 간의 매핑을 자동화해주는 기술
- ORM 프레임워크인 Hibernate, JPA (Java Persistence API)를 통해 객체와 데이터베이스 간의 매핑을 지원

Transaction Management (트랜잭션 관리)
- 선언적 트랜잭션 관리를 지원하여 애노테이션을 사용하여 트랜잭션을 정의하고 관리 가능
- 데이터 액세스 작업을 트랜잭션 단위로 묶어 예외 발생 시 롤백하거나 커밋하는 등의 작업을 자동화 가능

---

### 3. Web
- 웹 애플리케이션을 개발하기 위한 모듈
- 스프링 MVC와 같은 웹 프레임워크를 통해 웹 애플리케이션을 구축 가능
- RESTful 서비스를 개발하는 데도 사용

Spring WEB
- 기초적인 웹(Web)에 대한 부분들을 관리

Spring Servlet
- 웹 애플리케이션 실행을 위한 MVC 구현이 포함

Spring Web-Socket
- 웹 애플리케이션에서 양방향 통신을 구현하는 데 사용
- 서버와 클라이언트 간에 실시간으로 데이터를 통신 가능

--- 

### 4. 그 외 컴포넌트

Aspect-Oriented Programming (AOP)
- 관점 지향 프로그래밍을 지원하는 모듈
- 애플리케이션의 핵심 기능과 부가적인 기능(로깅, 보안, 트랜잭션 등)을 분리하여 모듈화
- 각 관심사를 개별적으로 다룰 수 있게함

Instrumentation
- 클래스로딩과 같은 자바 에이전트 기반의 애플리케이션 모니터링을 지원
  
Messaging
- 메시지 기반의 애플리케이션을 개발하기 위한 모듈
- 메시지 큐와 메시지 브로커를 통해 이벤트 기반 아키텍처를 구축



