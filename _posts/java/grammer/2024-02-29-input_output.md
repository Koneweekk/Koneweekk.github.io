---
title: JAVA - 입력과 출력
date: 2024-02-29 10:30:00 +09:00
categories: [Java, 문법]
tags:
  [
    프로그래밍 언어,
    Java,
    입력,
    출력
  ]
---

## Ⅰ. 문자열 출력

### ⅰ. println

줄바꿈이 포함된 문자열출력

```java
System.out.println("a");
System.out.println("b");
// a
// b
```

<hr>

### ⅱ. print

출력 후 줄바꿈하지 않음

```java
System.out.print("a");
System.out.print("b");
// ab
```

<hr>

### ⅲ. printf

정해진 형식에 입력값을 넣어 문자열을 출력  

`%`의 위치에 인자로 입력한 변수가 위치하여 입력됨  
```java
for (int i = 2; i < 10; i++) {
  for (int j = 1; j < 10; j++) {
    System.out.printf("%d*%d=%d\t", i, j, i*j);
  }
  System.out.println();
}
```

|리터럴|설명|
|---|---|
|`%d`|정수|
|`%f`|실수|
|`%s`|문자열|

<hr><br>

## Ⅱ. 문자열 콘솔 입력

### ⅰ. Scanner 클래스

문자열을 입력받기 위해선 `Scanner`클래스가 필요하다  
`import`를 통해 모듈을 불러와야한다.

그 후 `Scanner` 객체를 생성하고, 그 객체를 통해 입력받는다.

```java
import java.util.Scanner;

public class problem {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		...
	}
}
```

### ⅱ. nextLine

입력값을 문자열 타입으로 받는 메서드

```java
Scanner sc = new Scanner(System.in);

String value1 = sc.nextLine();
System.out.println("value1 : " + value1);
```

정수나 실수 타입으로 입력받고 싶다면 다음과 같은 메서드를 사용한다.

```java
// 정수 입력
int value2 = sc.nextInt();
System.out.println("value2 : " + value2);

// 실수 입력
float value3 = sc.nextFloat();
System.out.println("value3 : " + value3);
```