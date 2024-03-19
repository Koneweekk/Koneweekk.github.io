---
title: JAVA - 익명함수 (Anonymous Class)
date: 2024-03-19 15:00:00 +09:00
categories: [Java, 문법]
tags:
  [
    프로그래밍 언어,
    Java,
    익명 함수,
    일회용 함수
  ]
---

### 1. 익명 클래스란?

> 클래스 명을 선언해주지 않은 클래스

원칙을 무시하고 인터페이스나 추상 클래스의 추상자원의 직접 구현
- 이 클래스는 일회용으로 재사용이 불가능하다.

<hr>

### 2. 사용하는 이유

추상 클래스는 스스로 객체 생성 불가능
- 반드시 상속하여 강제 구현 후 사용하여야 한다.
  
그렇다면 추상 클래스를 상속하지 않고 사용할 수 있을까...?
- 일회성으로 사용되고 버려지는 클래스를 통해 추상 클래스를 상속하여 사용

<hr>

### 3. 사용하는 분야

1. 이벤트 처리 >> 웹(클릭, 더블클릭, 마우스 오버)
2. 스레드(Thread)
3. 람다식 (줄임 표현식)
4. 스트림 (대량의 데이터를 표준화된 방법으로 구현)

<hr>

### 4. 사용 코드

#### 추상 클래스의 직접 사용

```java
// 추상 클래스
abstract class Person { 
  abstract void eat();
}

public class ExAnonymous {

  public static void main(String[] args) {
    
    // 익명 클래스 객체 할당
    Person person = new Person() {
      // 직접 함수 오버라이드
      @Override
      void eat() {
        System.out.println("익명 객체 타입으로 구현");
        
      }
    };

    person.eat();  // 익명 객체 타입으로 구현
  }

}
```

#### 인터페이스 직접 사용

```java
// 인터페이스
interface Eatable{
  void eat();
}

class User{
  // 인터페이스를 매개변수로 받음
  void method(Eatable e) {
    e.eat();
  }
}


public class Ex15_InnerClass {

  public static void main(String[] args) {
    User user = new User();
    
    // 익명으로 인터페이스 구현
    user.method(new Eatable() {
      // 직접 함수 오버라이드
      @Override
      public void eat() {
        System.out.println("익명 객체 타입으로 구현");
      }
    }); // 익명 객체 타입으로 구성
  }

}
```