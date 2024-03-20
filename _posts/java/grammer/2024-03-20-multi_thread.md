---
title: JAVA - 멀티 스레드 (Multi Thread)
date: 2024-03-20 09:00:00 +09:00
categories: [Java, 문법]
tags:
  [
    프로그래밍 언어,
    Java,
    멀티스레드
  ]
---

## Ⅰ. 멀티 스레드의 개념

### 1. 프로세스란

> 운영체제에서 실행 중인 프로그램

운영체제가 멀티 태스킹을 실행하는 방법
- 멀티 프로세스를 생성
- 하나의 프로세스에서 멀티 스레드를 생성

<hr>

### 2. 스레드란

> 코드의 실행 흐름

스레드가 2 개라는건 2개의 코드 실행 흐름이 생긴다는 것
- 이러한 스레드가 한 프로세스에 여러 개 있으면 멀티 스레드

<hr>

### 3. 멀티 스레드와 멀티 프로세스

![멀티 스레드와 멀티 프로세스](https://user-images.githubusercontent.com/22395934/71546354-6c4f4d80-29d9-11ea-8784-5d3fc534e3ea.png)

멀티 프로세스
- 프로그램 단위의 멀티 태스킹
- 프로세스끼리 서로 독립적이므로 발생한 오류가 다른 프로세스에 영향 X

멀티 스레드
- 프로그램 내부에서의 멀티 태스킹
- 하나의 스레드가 예외를 발생시키면 프로세스 전체가 종료

<hr>

### 4. 활용 예시

1. 데이터를 분할해서 병렬로 처리
2. 안드로이드 앱에서 네트워크 통신
3. 다수 클라이언트 요청을 처리하는 서버

등 다양한 분야에 필수적으로 쓰인다.

<hr><br>

## Ⅱ. 스레드 생성과 실행

### 1. 메인 스레드

모든 자바 프로그램은 메인 스레드가 `main()` 메소드를 실행하며 시작
- 마지막 코드 혹은 `return`문을 만나면 종료

메인 스레드는 추가 작업 스레드를 만들어 실행 가능
- 이런 경우 실행 중이 스레드가 하나라도 있으면 프로세스 종료 X

<hr>

### 2. Thread 클래스를 통한 직접 생성

`Thread` 클래스로부터 작업 스레드 객체를 직접 생성 가능
- `start()` : memory call stack 생성 후 `run()` 함수를 stack에 올림

```java
Thread thread = new Thread(new Task());

thread.start()
```

이 때 `Runnable` 인터페이스를 구현하는 클래스만 작업 가능
- `run` 메서드를 재정의해서 오버라이드하여야한다.
- 구현 클래스는 스레드가 아닌 스레드가 될 수 있는 클래스이다.

```java
class Task implements Runnable{
  @Override
  public void run(){
    ...
  }
}
```

인터페이스를 구현하지 않고 익명 클래스로도 구현 가능
- 이 방법이 더 많이 사용됨

```java
Thread thread = new Thread(new Runnable() {
  @Override
  public void run(){
    ...
  }
});
```

<hr>

### 3. Thread 클래스 상속을 통한 생성 

`Thread` 클래스를 상속한 후 `run()` 메소드를 재정의

```java
class Task extends Thread {
  @Override
  public void run() {
    for (int i = 0; i < 1000; i++) {
      System.out.println("Task : " + i + this.getName());
    }
    System.out.println("Task 함수 종료");
  }
}
```

시작 방법 또한 `start()` 메서드로 동일

```java
Task task = new Task();

task.start();
```

구상 클래스를 구현하지 않고 익명 클래스를 통해서 구현 가능
- 이 방법이 더 많이 사용됨

```java
Thread thread = new Thread() {
  @Override
  public void run() {
    for (int i = 0; i < 1000; i++) {
      System.out.println("Task : " + i + this.getName());
    }
    System.out.println("Task 함수 종료");
  }
}

thread.start();
```

자바는 다중 상속을 지원하지 않는다.  
그러므로 이미 상속하고 있는 부모가 있는 클래스는 `Runnable` 인터페이스를 활용하자

<hr>

### 4. 스레드 이름

스레드는 자동으로 `Thread-n`이란 이름을 가짐
- 디버깅 목적으로 스레드에 원하는 이름을 붙여줄 수 있음
- `setName()` 메서드로 이름 변경
- `getName()` 메서드로 스레드의 이름 호출 가능

```java
public class Ex02_Multi_Thread {

  public static void main(String[] args) {
    Thread t = new Thread() {
      @Override
      public void run() {
        System.out.println(this.getName());
      }
    };
    t.setName("스레드");
    t.start();
    // 스레드
  }

}
```

<hr><br>

## Ⅲ. 스레드의 상태

### 1. 일시정지

`sleep` 메소드로 정해진 시간만큼 스레드를 일시정지 시킬 수 있다.
- `InterruptedException`이 발생할 수 있으므로 예외 처리를 해줘야함

```java
class WordTime extends Thread {

  @Override
  public void run() {
    for (int i = 10; i > 0; i--) {
      
      if (Ex04_Multi_Thread_Game.inputCheck) return;
      
      try {  // sleep 메서드는 예외 처리 필수
        System.out.println("남은 시간 : " + i);
        Thread.sleep(1000);  // sleep을 통해 각 반복문마다 1초 대기
      } catch (Exception e){
        System.out.println(e.getMessage());
      }
    }
  }

}
```

<hr>

### 2. 다른 스레드 종료 대기

다른 스레드가 종료될 때까지 대기하고 싶다면 `join()` 함수를 사용

```java
WordTime timer = new WordTime();
timer.start();

WordInputThread wordInput = new WordInputThread();
wordInput.start();

try {
  timer.join();  // timer 스레드가 종료될 때까지 대기
  wordInput.join();  // wordInput 스레드가 종료될 때까지 대기
} catch (Exception e) {
  System.out.println(e.getMessage());
}

// 두 스레드가 모두 종료되어야 출력
System.out.println("main end");
```

<hr><br>

## Ⅳ. 동기화

### 1. 동기화란?

- 멀티스레드 환경에서 공유된 자원에 대한 접근을 조절
- 여러 스레드가 해당 자원에 접근할 때 데이터의 일관성을 유지

<hr>

### 2. 동기화 사용법

객체 동기화가 있고 함수 동기화가 있다.
- 함수 동기화가 주로 쓰임
- 함수 타입 앞에 `synchronized` 키워드 입력

이를 통해 이 함수에 동시 접근을 차단할 수 있다.

```java
class Resource {
  // 동기화 함수 선언
  synchronized void start(String name) {  
    System.out.println(name + " 님 자원 사용 시작");
    for (int i = 0; i < 100; i++) {
      System.out.println(name + "님 사용 중 " + i);
    }
    System.out.println(name + "님 자원 사용 종료");
  }
}

public class SyncEx {

  public static void main(String[] args) {
    Resource r = new Resource();
    
    User kim = new User(r, "김씨");
    User lee = new User(r, "이씨");
    User park = new User(r, "박씨");
    
    kim.start(); 
    lee.start();
    park.start();
    // kim > lee > park 이 순서로 start 함수 실행 보장
  }

}
```

<hr><br>

## 참고
- [https://webcoding-start.tistory.com/62](https://webcoding-start.tistory.com/62)