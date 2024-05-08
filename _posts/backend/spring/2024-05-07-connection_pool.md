---
title: Spring - Connection Pool 
date: 2024-05-07 09:30:00 +09:00
categories: [Back-End, Spring]
tags: [Back-End, Java, Spring, Connection Pool ]
---

## Ⅰ. 커넥션 풀

### 1. 커넥션 풀이란?

> 데이터베이스와의 연결을 관리하는 데 사용되는 소프트웨어 구성 요소

- 데이터베이스 연결을 생성하고 폐기하는 과정은 비용이 많이 드는 작업 중 하나
- 매번 연결을 만들고 제거하는 대신에 이러한 연결을 사전에 만들어 두고 재사용함으로써 성능을 향상
- 커넥션 풀을 사용함으로써 데이터베이스와의 연결 관리가 효율적으로 이루어짐
- 애플리케이션의 성능이 향상되고 데이터베이스 서버의 부하가 감소

---

### 2. 커넥션 풀의 사용 단계

연결 생성
- 초기화 단계에서 일정 수의 데이터베이스 연결을 만든다.
  
연결 대여
- 애플리케이션에서 데이터베이스에 연결이 필요할 때, 커넥션 풀에서 사용 가능한 연결을 제공한다.

연결 반환
- 애플리케이션이 데이터베이스 작업을 완료한 후, 연결을 다시 풀로 반환한다.
- 이 연결은 다시 사용될 수 있다.

풀 관리
- 풀이 사용 가능한 연결의 수를 유지
- 만료된 연결을 재생성하거나 제거하여 풀의 성능을 최적화

---
<br>

## Ⅱ. 히카리 CP

### 1. HikariCP란?

> Java 언어용으로 작성된 빠르고 경량의 JDBC 커넥션 풀 라이브러리

높은 성능
- HikariCP는 초당 수천 개의 연결 요청을 처리할 수 있다,
- 이는 다른 커넥션 풀 라이브러리에 비해 뛰어난 성능을 보장

경량화
- HikariCP는 매우 적은 메모리와 CPU 리소스를 사용하므로 높은 확장성을 제공
- 이는 애플리케이션의 메모리 효율성과 성능을 향상시킴
  
자동 구성
- 대부분의 경우에는 추가 구성 없이도 기본 설정으로 충분히 운영이 가능

호환성
- JDBC 4.0 이상을 지원하는 모든 Java 애플리케이션과 호환

---

### 2. 설정 세팅

Maven 기준으로 설명드리겠습니다.

oom.xml 파일에 의존성 주입

```xml
<dependency>
    <groupId>com.zaxxer</groupId>
    <artifactId>HikariCP</artifactId>
    <version>4.0.3</version> <!-- 버전은 최신 버전으로 업데이트 가능 -->
</dependency>
```

root-config.xml에 빈 정의

```xml
<!-- HikariCP의 구성을 정의하는 빈-->
<bean id="hikariConfig" class="com.zaxxer.hikari.HikariConfig">
  <!-- spy를 사용하기 위한 설정 -->
  <property name="driverClassName"
    value="net.sf.log4jdbc.sql.jdbcapi.DriverSpy"></property>
  <property name="jdbcUrl"
    value="jdbc:log4jdbc:oracle:thin:@localhost:1521:XE"></property>
  <property name="username" value="springuser"></property>
  <property name="password" value="1004"></property>
</bean>

<!-- HikariCP의 DataSource 구현체를 정의하는 빈-->
<bean id="driverManagerDataSource"
  class="com.zaxxer.hikari.HikariDataSource" destroy-method="close">
  <constructor-arg ref="hikariConfig"></constructor-arg>
</bean>
```

---

### ※ spy란?

- 데이터베이스 커넥션의 활동을 모니터링하고 기록하는 기능을 의미
- 이를 통해 커넥션의 생성, 사용, 반환 등의 정보를 추적 가능
- 주로 성능 모니터링, 디버깅, 문제 해결 등을 위해 사용

위 코드처럼 DriverSpy를 사용하여 데이터베이스에 연결하면, Log4jdbc가 JDBC 호출을 가로채고 로깅 가능

