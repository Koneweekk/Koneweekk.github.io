---
title: Spring - Bean 객체의 생명 주기
date: 2024-04-29 15:30:00 +09:00
categories: [Back-End, Spring]
tags: [Back-End, Java, Spring, Bean, Life cycle]
---

## 1. Bean 객체의 생명 주기란?

> 해당 빈이 생성되고 초기화되는 과정부터 제거되는 과정까지의 전체 과정을 의미

개발자는 초기화 작업을 수행하기 위해 다음과 같은 메서드를 오버라이드 가능
- `@PostConstruct` 어노테이션이 지정된 메서드
- `InitializingBean` 인터페이스의 `afterPropertiesSet()` 메서드

개발자는 빈이 소멸되기 전에 정리 작업을 수행하기 위해 이러한 메서드를 오버라이드 가능
- `@PreDestro` 어노테이션이 지정된 메서드
- `DisposableBean` 인터페이스의 `destroy()` 메서드

---

## 2. 인터페이스를 통한 라이플 사이클 설정

```java
package spring;

import org.springframework.beans.factory.DisposableBean;
import org.springframework.beans.factory.InitializingBean;

public class Client implements InitializingBean , DisposableBean {
  ...
	
	//초기화
	@Override
	public void afterPropertiesSet() throws Exception {
		System.out.println("Client 초기화 함수 호출");		
	}

	//소멸 (객체가 소멸될때 호출되는 함수)
	@Override
	public void destroy() throws Exception {
		System.out.println("Client 소멸자 함수 호출");
		
	}

}
```

---

## 3. xml을 통한 라이프사이클 설정

```java
package spring;


public class Client2 {
  ...
	
	//개발자가 임의로 만든 함수 > 객체의 초기화
	public void Client2_init(){
		System.out.println("사용자 정의 초기화 함수 호출");
	}
	//개발자가 임의로 만든 함수 > 객체의 소멸자
	public void Client2_close(){
		System.out.println("사용자 정의 소멸자 함수 호출");
	}

}
```

```xml
<!-- init-method와 destroy-method에 빈 생성 및 삭제시 실행될 함수 지정 가능 -->
<bean id="client2" class="spring.Client2"
  init-method="Client2_init" destroy-method="Client2_close" >
  <constructor-arg value="192.168.0.115" />
  <property name="host" value="192.168.1.1" />
</bean>
```