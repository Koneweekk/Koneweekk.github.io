---
title: Spring - 어노테이션(Annotation)
date: 2024-04-29 14:40:00 +09:00
categories: [Back-End, Spring]
tags: [Back-End, Java, Spring, Annotation]
---

## Ⅰ. @Autowired

### 1. @Autowired이란?

> 의존관계를 자동설정할 때 사용하며 타입을 이용하여 의존하는 객체를 삽입

- 컨테이너 안에 해당 타입 빈객체를 찾아서 자동 주입
- 해당 타입의 빈객체가 존재하지 않거나 또는 2개 이상 존재할 경우  스프링은 예외를 발생

---

### 2. 선행 설정

- 설정 위치 : 생성자, 필드, 메소드(굳이 setter메소드가 아니여도 된다)
- 추가설정 : `AutowiredAnnotationBeanPostProcessor` 클래스를 빈으로 등록시켜줘야 한다. 
- 해당 설정 대신에 `<context:annotation-config>` 태그를 사용해도 된다.

---

### 3. 매개변수 required

- `@Autowired` 어노테이션을 적용한 프로퍼티에 대해 굳이 설정할 필요가 없는 경우 `false`값을 줌
- 이때 해당 프로퍼티가 존재하지 않더라도 스프링은 예외를 발생시키지 않는다. 디폴트값은 `true`
- `@Autowired(required = true)` : 무조건 자동 주입(default 설정)
-	`@Autowired(required = false)` : 컨테이너 안에 원하는 타입 객체가 없으면 주입 X


---

### 4. context:annotation-config

Annotation 사용에 필요한 모든 클래스를 한방에 컨테이너안에 생성
- 장점 : 각각의 annotation 에 대해서 일일인 선행 빈객체를 만들 필요없다
- 단점 : 사용하지 않는 선행 빈객체도 등록한다
		
---

### 5. @Autowired 정상 동작하지 않는 경우

injection 타입 객체가 IOC 컨테이너 안에 없는 경우
- `org.springframework.beans.factory.NoSuchBeanDefinitionException`

같은 타입의 빈객체가 여러가 있는 경우 
- `org.springframework.beans.factory.NoUniqueBeanDefinitionException`

```xml
<bean id="a"  class="DI_Annotation_01.Recorder"></bean>
<bean id="b"  class="DI_Annotation_01.Recorder"></bean>
<bean id="c"  class="DI_Annotation_01.Recorder"></bean>
```
		 
여러개의 빈객체가 있어도(같은 타입) id 값이 setter 함수 parameter명과 동일하면 자동 주입 성공 

---

### 6. 사용 예시

`setRecorder`에 `@Autowired` 어노테이션을 추가
- `recorder` 타입의 bean 객체가 ioc 컨테이너에 존재한다면 자동 주입

```java
import org.springframework.beans.factory.annotation.Autowired;

public class MonitorViewer {
	private Recorder recorder;

	public Recorder getRecorder() {
		return recorder;
	}

	@Autowired
	public void setRecorder(Recorder recorder) {
		this.recorder = recorder;
	}
}
```

id가 `recorder`인 bean객체가 존재하므로 자동 주입됨

```xml
<context:annotation-config />
<bean id="recorder"  class="DI_Annotation_01.Recorder"></bean>
<bean id="monitorViewer"  class="DI_Annotation_01.MonitorViewer"></bean>

<bean id="a"  class="DI_Annotation_01.Recorder"></bean>
<bean id="b"  class="DI_Annotation_01.Recorder"></bean>
<bean id="c"  class="DI_Annotation_01.Recorder"></bean>
```

---
<br>

## Ⅱ. @Qualifier

### 1. @Qualifier란?

- `@Autowired`의 목적에서 동일 타입의 빈객체가 존재시 특정빈을 삽입할 수 있게 설정
- `@Autowired` 어노테이션과 함께 사용
동일타입의 빈객체 설정에서 `<qualifier value="[alias명]"/>`를 추가

---

### 2. 사용 예시

`@Autowired`와 `@Qualifier`를 함께 지정
- 동일 타입의 bean 객체가 존재할 시 어떤 객체를 할당할지 지정 가능

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;

public class MonitorViewer {
	private Recorder recorder;
	
	public Recorder getRecorder() {
		return this.recorder;
	}

	@Autowired
	@Qualifier("recoder_1")
	public void setRecorder(Recorder recorder) {
		this.recorder = recorder;
		System.out.println("setRecorder : " + this.recorder);
	}
	
	@Autowired
	@Qualifier("recoder_2")
	public void gMethod(Recorder rec) {
		System.out.println("Gmethod(rec) : " + rec);
	}
}
```

각 `Reccorder` 타입 bean 객체에 `qualifier` 지정

```xml
<bean id="monitorViewer"  class="DI_Annotation_02.MonitorViewer"></bean>

<bean id="xx" class="DI_Annotation_02.Recorder">
    <qualifier value="recoder_1"></qualifier>
</bean>
<bean id="yy" class="DI_Annotation_02.Recorder">
    <qualifier value="recoder_2"></qualifier>
</bean>
```

---
<br>

## Ⅲ. @Required

### 1. @Required란?

- 필수 프로퍼티를 지정 
- 설정 위치 : setter메소드
- `RequiredAnnotationBeanPostProcessor` 클래스를 빈으로 등록시켜줘야 한다. 
- 해당 설정 대신에 `<context:annotation-config>` 태그를 사용해도 된다.
  
---

### 2. 사용 예시

```java
class User{
 private String name;
 
 // 반드시 이 프로퍼티를 할당하겠다고 선언
 @Required  
 public setName(String name){
    this.name = name; 
 }

}
```

```xml
<context:annotation-config>
<bean id="user" class="DI_Annotation_03.User">
    <!-- 아래 부분이 있어야 @Required 어노테이션 만족 -->
    <property name="name" value="kglim"></property>
</bean>
```

---
<br>

## Ⅳ. @Resource

### 1. @Resource란?

- 필요로 하는 자원을 자동 연결(의존하는 빈 객체 전달)할 때 사용
- `@Autowired`와 같은 기능을 하며 
- 이름으로(by name)으로 연결시켜준다는 것에서 차이점이 있음
- 설정위치 : 프로퍼티, setter메소드
- `CommonAnnotationBeanPostProcessor` 클래스를 빈으로 등록시켜줘야 한다. 
- 해당 설정 대신에 `<context:annotation-config>` 태그를 사용해도 된다.

---

### 2. 사용 예시

```java
import javax.annotation.Resource;

/*
JAVA 1.9 버전부터 @Resource 사용하기 위해서 
javax.annotation-api-1.3.2.jar 파일 추가 해야 합니다 

maven
<dependency>
	<groupId>javax.annotation</groupId>
	<artifactId>javax.annotation-api</artifactId>
	<version>1.3.1</version>
</dependency>
*/

public class MonitorViewer {
	private Recorder recorder;
	
	public Recorder getRecorder() {
		return this.recorder;
	}
	
	@Resource(name="yy") //같은 타입의 객체가 있을때 name 값으로 찿는다
	public void setRecorder(Recorder recorder) {
		this.recorder = recorder;
		System.out.println("setRecorder : " + this.recorder);
	}
	
}
```

- `@Resource(name="yy")` : 해당 타입 중 `yy`란 id를 가진 bean 객체를 주입

```xml
<context:annotation-config />
<bean id="monitorViewer"  class="DI_Annotation_03.MonitorViewer"></bean>

<bean id="xx"  class="DI_Annotation_03.Recorder"></bean>
<bean id="yy"  class="DI_Annotation_03.Recorder"></bean>
<bean id="zz"  class="DI_Annotation_03.Recorder"></bean>
```

---
<br>

## Ⅴ. Java Config

### 1. @Configuration

> 자바 기반의 설정 클래스를 지정하는 어노테이션

- xml을 자바 파일로 대체하는 어노테이션
- xml 태그 대신 함수를 통해 구현

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class ConfigContext {
  ...
}
```

- `AnnotationConfigApplicationContext`를 통해 ioc 컨테이너 선언

```java
ApplicationContext  context = new AnnotationConfigApplicationContext(ConfigContext.class);
User user = context.getBean("user",User.class);
user.userMethod();

User2 user2 = context.getBean("user2",User2.class);
user2.userMethod2();
```

---

### 2. @Bean

- Spring 빈을 정의할 때 사용
- 함수를 통해 bean 객체 생성

```java
@Configuration  
public class ConfigContext {

	//xml <bean id="user" class="DI_Annotation_04_javaconfig.User">
	@Bean
	public User user() {
		return new User();
	}
	
	//xml <bean id="user2" class="DI_Annotation_04_javaconfig.User2">
	@Bean
	public User2 user2() {
		return new User2();
	}
}
```

---



