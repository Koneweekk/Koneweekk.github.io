---
title: JAVA - 기본 작동 원리
date: 2024-02-27 15:30:00 +09:00
categories: [Java, 문법]
tags:
  [
    프로그래밍 언어,
    Java,
    작동원리,
    JVM
  ]
---

## Ⅰ. Java의 기본 실행 과정

### ⅰ. `.java`

`.java`은 자바 소스 파일의 확장자명이다.  
텍스트파일이므로 어떤 텍스트 에디터도 작성 가능하다.

```java
// Hello.java

class Hello {
  public static void main(String[] args) {
    System.out.prinln("Hello world");
  }
}
```

<hr>

### ⅱ. `.class`

`javac` 명령어는 소스 파일을 컴파일 하는데,  
이 경우 `.class` 확장자를 가진 파일이 생성된다.

이 파일이 <u>바이트코드 파일</u>로  
mac, window, linux 모두 동일한 소스 파일이면 동일한 바이트코드 파일이 생성된다.

<hr>

### ⅲ. JVM

> `.class` 파일을 완벽한 기계어로 번역하고 실행시키는 자바가상머신

`java`란 명령어로 JDK와 함께 설치된 JVM을 구동시켜 `.class` 파일을 실행시킨다.  

바이트코드 파일과 달리 자바 가상 머신은 운영체제마다 다르다.  
각 운영체제별로 이해하는 기계어로 번역해야하기 때문이다.  
그래서 운영체제별로 설치하는 JDK가 다르다.

![자바구동원리](https://blog.kakaocdn.net/dn/tmLpv/btrrQsh23RM/VyS1jhoUtcszuemDz6dZH1/img.jpg)

<hr><br>

## Ⅱ. JVM의 기본 구조

### ⅰ. JVM의 구조

![JVM](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcQRqku%2Fbtru0vJ6Ixx%2F9qCTW7ChXc80fGfQUrT4B0%2Fimg.png)

#### 1. Class Loader

필요한 클래스 파일을 로드하여 Runtime Data Area에 적재한다.
클래스 파일을 동적으로 로드하여 실행 시점에 필요한 클래스를 사용할 수 있도록 한다.

#### 2. Runtime Data Area

JVM은 메모리를 여러 영역으로 나누어 관리한다.

- Method Area : 클래스 정보, 상수, 정적 변수 등이 저장
- Heap : 객체 인스턴스가 저장되는 공간으로, 가비지 컬렉션에 의해 관리됨
- Stack : 메소드 호출과 관련된 데이터(로컬 변수, 메소드 호출 및 리턴 값 등)가 저장
- Program Counter Register: 현재 실행 중인 스레드의 실행 위치를 저장
- Native Method Stack : 자바 이외의 언어로 작성된 네이티브 코드를 실행할 때 사용
#### 3. Execution Engine

바이트 코드를 실제로 실행한다.

보통 인터프리터와 JIT 컴파일러의 조합으로 구성됩니다.
- 인터프리터 : 바이트 코드를 한 줄씩 읽어 실행합니다.
- JIT(Just-In-Time) 컴파일러 : 인터프리터가 반복되는 코드를 컴파일하여 네이티브 코드로 변환하여 실행 속도를 향상시킵니다.

<hr>

### ⅱ. JVM의 동작 방식

JVM의 실행순서는 다음과 같습니다.

1. 클래스 로더를 통해 필요한 클래스 파일들을 메모리에 로드.

2. 클래스 파일들은 검증 단계를 거쳐 올바른 형식인지 확인. 이는 자바 프로그램의 안전성을 보장하기 위한 중요한 단계.

3. 클래스의 정적 변수를 위한 메모리 공간을 할당하고 기본값으로 초기화.

4. 스레드를 생성하고, 스레드가 시작되면 해당 스레드가 작성된 바이트 코드를 해석하고 실행

5. 프로그램 실행이 완료되면 더 이상 필요하지 않은 클래스들은 메모리에서 제거

<hr><br>

## 참고자료

- [https://devpad.tistory.com/56](https://devpad.tistory.com/56)
- [https://coding-factory.tistory.com/828](https://coding-factory.tistory.com/828)