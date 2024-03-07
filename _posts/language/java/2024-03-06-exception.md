---
title: JAVA - 예외(Exception)
date: 2024-03-06 09:20:00 +09:00
categories: [프로그래밍 언어, Java]
tags:
  [
    프로그래밍 언어,
    Java,
    예외 처리,
    오류
  ]
---

## Ⅰ. 예외 클래스

### 1. 에러와 예외

#### 에러(error)

> 하드웨어의 고장으로 인해 응용프로그램 실행 오류가 발생하는 것

개발자가 이런 에러에 대처할 방법이 전혀 없다.

#### 예외(exception)

> 잘못된 사용 또는 코딩으로 발생하는 오류

오류와 같이 프로그램이 곧바로 종료되지만  
예외 처리르 통해 실행 상태를 유지할 수 있다.

<hr>

### 2. 예외의 종류

#### 일반 예외(Exception)

> 컴파일러가 예외 처리 코드 여부를 검사하는 예외

이러한 예외가 발생하면 실행 파일 자체가 만들어지지 않는다.

#### 실행 예외(Runtime Exception)

> 컴파일러가 예외 처리 코드 여부를 검사하지 않는 예외

문제가 생긴 시점에서 프로그램이 강제 종료되고 이후 코드가 실행이 멈춘다.

<hr>

### 3. 예외 클래스

![예외 클래스](https://www.tcpschool.com/lectures/img_java_exception_class_hierarchy.png)

자바는 예외가 발생하면 예외 클래스로부터 객체를 생성하여 예외  
이 객체는 예외 처리 시 사용된다.

모든 에러와 예외 클래스는 `Throwable` 클래스를 상속

예외 클래스는 `java.lang.Exception` 클래스를 상속

실행 예외는 `RuntimeException`와 그 자식 클래스에 해당

<hr><br>

## Ⅱ. 예외 처리 방법

### 1. 예외 처리 코드

> 예외가 발생했을 때 프로그램 종료를 막고 정상 실행 유지를 위한 코드

`try{} catch{} finally{}` 블록으로 코드 구성

![try catch](https://velog.velcdn.com/images%2Fjwoo%2Fpost%2F19a27cd4-87e7-4c19-9399-4a5a1a3554f7%2FUntitled%201.png)

- `try` 블록이 정상 실행되면 `catch`을 넘어가고 `finally`를 실행
- `try` 블록에서 예외 발생하면 실행을 중단하고 `catch`을 실행 후 `finally` 실행
- `finally` 블록은 `return`을 만나도 강제로 실행된다

```java
import java.io.IOException;

public class Ex03_Finally {
  
  static void copyFiles() {
    System.out.println("copy files");
  }
  
  static void startInstall() {
    System.out.println("install...");
  }
  
  static void deleteFile() {
    System.out.println("복사했던 파일 삭제");
  }

  public static void main(String[] args) {
    try {
      copyFiles();
      startInstall();
      throw new IOException("Install 도중 문제 발생...");
    } catch (IOException e) {
      System.out.println(e.getMessage());
      return;
    } finally {
      // 강제 실행 블럭
      // return이 앞에 있어도 반드시 실행됨
      deleteFile();
    }

  }

}
```

### 2. 다중 예외 처리

다중 `catch`문으로 예외 종류에 따라 예외 처리 코드를 다르게 작성할 수 있다.

`try` 블록에서 하나의 예외가 발생하면  
즉시 실행이 멈추고 `catch`로 이동하기 때문에 다발적으로 예외가 발생하지 않는다.

```java
try {
  IOException 발생  // 첫 번째 catch로 이동
  NullPointerException 발생  // 두 번째 catch로 이동
} catch(IOException e) {
  ...
} catch(NullPointerException e) {
  ...
}
```

상위 예외 클래스도 검사에 걸리기 때문에 상위 예외 클래스는 아래에 넣어야 한다.

```java
// Exception은 최상위 예외 클래스
try {
  IOException 발생  // 첫 번째 catch로 이동
  NullPointerException 발생  // 첫 번째 catch로 이동
} catch(Exception e) {
  ...
} catch(IOException e) {
  ...
} catch(NullPointerException e) {
  ...
}
```

<hr>

### 3. throws

`throws` 키워드를 통해 메소드를 호출한 곳으로 예외를 떠넘길 수 있다.

메소드에서 해당 예외를 처리하지 않고 넘겼기 때문에  
메소드를 호출하는 곳에서 예외를 반드시 처리해주어야 한다.
그렇지 않을 경우 에러가 발생한다.

```java
class ExClass {
  ExClass(String path) throws IOException, NullPointerException {}
}

public class Ex04_Throw {
	public static void main(String[] args) {  // 컴파일 에러
		ExClass ex = new ExClass("C:\\Temp");
	}
}
```




<hr><br>

## 사용자 정의 예외

### 1. 사용자 정의 예외 만들기

사용자가 예외 상황이라고 생각되는 상황을 임의로 예외로 선언하는 것이다.

통상적으로 일반 예외는 `Exception`의 자식 클래스로,  
실행 예외는 `RuntimeException`의 자식 클래스로 선언한다.

```java
public class userException extends Exception {
  // 기본 생성자
  public userException() {

  }

  // 예외 메시지 입력 생성자
  public userException(String message) {
    super(message);
  }
}
```

보편적으로 기본 생성자와 예외 메시지를 입력받는 생성자를 선언
- 공통 메소드인 `getMesaage()`의 리턴값으로 사용하기 위함

### 2. 예외 발생시키기

사용자 정의 예외를 직접 코드에서 발생시킬 수도 있다.

이 때 `throw` 라는 키워드와 예외 객체를 제공한다.

`try-catch`에서 직접 `throw` 예외를 처리할 수도 있고

```java
void method() {
  try {
    ...
    throw new Exception("exception");
  } catch (Exception e) {
    System.out.println(e.getMessage());
  }
}
```

메소드를 호출한 곳에서 예외를 처리하게 할 수도 있다.

```java
void method() throws Exception {
  ...
  throw new Exception("exception");
  ...
}
```

보편적으로 후자의 방법을 사용한다.

<hr><br>

## 참고
- [https://www.tcpschool.com/java/java_exception_class](https://www.tcpschool.com/java/java_exception_class)
- [https://velog.io/@jwoo/after-java-semina-4](https://velog.io/@jwoo/after-java-semina-4)

