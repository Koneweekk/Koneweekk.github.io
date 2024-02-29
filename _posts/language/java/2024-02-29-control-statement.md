---
title: JAVA - 조건문과 반복문
date: 2024-02-29 09:00:00 +09:00
categories: [프로그래밍 언어, Java]
tags:
  [
    프로그래밍 언어,
    Java,
    조건문,
    반복문
  ]
---

## Ⅰ. 조건문

### ⅰ. if 문

`if (조건문) {}` 혹은 `if (조건문) 명령문` 형식으로 사용한다
```java
int value = 10
if (value % 2 == 0) {
  System.out.println("짝수")
}
```

`else`를 활용하여 나머지 경우를 처리할 수 있다.
```java
int value = 10
if (value % 2 == 0) {
  System.out.println("짝수")
} else {
  System.out.println("홀수")
}
```

`else if`를 활용하여 다수의 조건을 처리할 수 있다.  
하나의 조건문이라도 실행되면 그 아래 `else if` 혹은 `else`는 실행되지 않는다.
```java
int score = (int)((Math.random()*100) + 1);

String grade;
if (score >= 90) {
  grade = "A";
  grade += score < 95 ? "-" : "+";
} else if (score >= 80) {
  grade = "B";
  grade += score < 85 ? "-" : "+";
} else if (score >= 70) {
  grade = "C";
  grade += score < 75 ? "-" : "+";
} else if (score >= 60) {
  grade = "D";
  grade += score < 65 ? "-" : "+";
} else grade = "F";

System.out.println("당신의 학점은 " + grade + "입니다.");
```

<hr>

### ⅱ. switch 문

`if`의 조건문 대신 특정 변수의 값에 따라 실행문이 결정된다.

`switch`문의 괄호 안에는 정수 타입, 문자열 타입, `enum`타입의 변수를 사용할 수 있다.

```java
char data = 'A';

switch (data) {
  case 'A' : System.out.println("A입니다.");
    break;
  case 'B' : System.out.println("B입니다.");
    break;
  case 'C' : System.out.println("C입니다.");
    break;
  default : System.out.println("아무것 아닙니다.");
}
```

만약 만족하는 `case`에 `break`가 없다면 다음 `case`가 연달아 실행된다.
이러한 특성을 활용하여 효율적인 코드 작성이 가능하다.

```java
int jumsu = (int)((Math.random()*10) + 1) * 100;
		
String prize = "";
switch (jumsu) {
case 1000: prize += "TV , ";
case 900: prize += "NoteBook , ";
case 800: prize += "냉장고 , ";
case 700: prize += "한우세트 , ";
case 600: prize += "휴지";
  break;
default: prize = "칫솔";
  break;
}

System.out.println(prize + "에 당첨되셨습니다.");
```

`default`의 경우 필요없다면 생략가능하다.

<hr>

### ⅲ. ※ 문자열의 값 비교

문자열의 비교에서 `==`은 의외의 결과가 나오는 경우가 있다.

```java
String value1 = "abc";
String value2 = "abc";

System.out.println(value1 == value2);  // false
```

`String`은 참조 타입의 성격을 가지고 있기 때문이다.


그러므로 문자열을 비교할 땐 `equals()`메서드를 사용하는 것이 좋다.

```java
String value1 = "abc";
String value2 = "abc";

System.out.println(value1.equals(value2));  // true
```

<hr><br>

## Ⅱ. 반복문

### ⅰ. for 문

정해진 횟수동안 실행문을 반복하기 위해 사용한다.  
`for(초기화식;조건식;증감식){실행문}` 형식으로 사용한다.

1. 초기화식을 실행한다.
2. 조건식을 판단한다
3. 조건식이 `true`일 경우 실행문 실행
4. 증감식 실행
5. 2~5를 반복하다 조건식이 `false`가 되면 반복문 종료

```java
int result1 = 0;
for (int i = 1; i <= 100; i++) {
  result1 += i;
}
System.out.println(result1);
```

증감식 또한 다양한 방식으로 설정해줄 수 있다.
```java
int result2 = 0;
for (int i = 1; i <= 10; i+=2) {
  result2 += i;
}
System.out.println(result2);
```

<hr>

### ⅱ. while 문

정해진 횟수만큼이 아닌 조건만 맞다면 계속해서 반복할 수 있다.

`while (조건문) {실행문}` 형식으로 사용한다.

1. 조건문 판단
2. `true`이면 실행문 실행
3. 1 ~ 2를 반복하다 조건문이 `false`이면 종료

```java
int sum = 0;
int num = 1;
while (num <= 100) {
  sum += num;
  num ++;
}
System.out.println(sum);
```

조건을 제대로 걸어주지 않으면 실행문이 무한 반복되니 주의하자
```java
int sum = 0;
int num = 1;
while (num <= 100) {
  sum += num;
  // num ++;  이 구문이 빠지면 num <= 100은 계속 true
}
System.out.println(sum);
```

<hr>

### ⅲ. do while 문

실행문을 먼저 실행 후 조건문을 따져보는 반복문이다.

`do {실행문} while (조건문)`의 형식으로 이루어지며

1. 실행문 실행
2. 조건문 판단
3. 조건문 `true`면 1번으로 돌아감, `false`면 종료

```java
// do while
Scanner sc = new Scanner(System.in);

int inputData = 0;
do {
  System.out.println("숫자를 입력해주세요(0 ~ 9)");
  inputData = Integer.parseInt(sc.nextLine());
} while(inputData >= 10);
```

### ⅳ. 반복문 제어

#### break

`for`이나 `while`문을 탈출하거나,  
`switch`를 종료하는데 사용된다.

```java
int sum = 0;
int n = 1;

while(true) {
  sum += n;
  n++;
  if (n > 10) break;
}
```

만약 중첩된 반복문을 탈출하고자 할 때는 label을 이용한다.  
`break 라벨명`으로 원하는 반복문을 탈출할 수 있다.

```java
outer : for (int i = 0; i < 10; i++) {
  for (int j = 0; j < 10; j++) {
    if (j == 5) break outer;
  }
}
```

#### continue

실행문을 더 이상 진행하지 않고 다음 반복으로 넘어갈 때 사용한다.

```java
int sum = 0;

for (int i = 1; i < 10; i++) {
  if ( i % 2 == 0) continue;
  sum += i;
}
```

