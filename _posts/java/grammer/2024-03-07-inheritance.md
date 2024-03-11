---
title: JAVA - 상속(Inheritance)
date: 2024-03-07 09:00:00 +09:00
categories: [Java, 문법]
tags:
  [
    프로그래밍 언어,
    Java,
    클래스,
    상속
  ]
---

## Ⅰ. 상속이란?

### 1. 상속의 개념

> 부모 클래스의 필드와 메소드를 자식 클래스에게 물려주는 것

#### 장점

재사용성의 증가
- 이미 잘 개발된 클래스를 재사용해서 새로운 클래스를 만들기 때문
- 중복된 코드를 줄일 수 있음

수정의 최소화
- 부모 클래스를 수정하면 모든 자식 클래스에 수정 효과를 가져옴

#### 단점

커플링의 문제점
- 부모클래스와 자식클래스의 관계성 때문에 변화에 취약하다.

<hr>

### 2. 클래스의 상속

자식 클래스가 부모 클래스를 선택해서 상속받음

`public class 자식클래스 extends 부모클래스 {}`의 형식으로 사용한다.

```java
class GrandFather {
  public int gmoney = 5000;
}

class Father extends GrandFather {
  public int fmoney = 3000;
}

class Child extends Father {
  public int cmoney = 1000;
}

public class ChildEx {

  public static void main(String[] args) {
    Child child = new Child();
    System.err.println(child.gmoney);
    System.err.println(child.fmoney);
    System.err.println(child.cmoney);
  }

}
```

<hr>

상속시 메모리에는 상위 클래스부터 최하위 클래스까지 차례로 등록된다.  
이 때 최상위 클래스 `Object`는 `GrandFather` 클래스만 상속받는다.

만약 계층적 상속 과정에서 중복되는 자원 이름의 문제가 발생할 수도 있다.

### 3. 클래스의 다중 상속

다른 언어와 달리 JAVA는 다중 상속이 아닌 계층적 상속만 허용한다.

다중 상속을 위해선 `interface`를 사용해야한다.

```java
class GrandFather {
  public int gmoney = 5000;
}

class Father extends GrandFather {
  public int fmoney = 3000;
}

class Child extends GrandFather, Father {  // 컴파일 에러
  public int cmoney = 1000;
}
```

<hr>

### 4. 상속 관계에서의 private

상속관계라도 자식 클래스가 부모 클래스의 모든 자원에 접근할 수 있는 건 아니다.  
`private`로 선언된 자원엔 접근 불가능하다.

`public`으로 된 별도의 메서드를 만들어 접근해야한다.

```java
class GrandFather {
  public int gmoney = 5000;
  private int pmoney = 10000;
}

class Father extends GrandFather {
  public int fmoney = 3000;
}

class Child extends Father {
  public int cmoney = 1000;
}

public class ChildEx {

  public static void main(String[] args) {
    Child child = new Child();
    System.err.println(child.gmoney);
    System.err.println(child.fmoney);
    System.err.println(child.cmoney);
    System.err.println(child.pmoney);  // 컴파일 에러
  }

}
```

<hr><br>

## Ⅱ. 상속의 사용

### 1. 메소드 오버라이딩

> 상속관계에서 자식이 부모의 함수를 재정의하는 것

메소드 오버라이딩시 부모의 메소드는 숨겨지고, 자식 메소드가 우선 실행된다.

- 함수 이름, 리턴 타입, 매개변수 타입은 변화시킬 수 없다.
  - 블록 안 실행문에만 변화를 줄 수 있다.
- 접근 제한을 더 강하게 오버라이딩할 수 없다.
- 세로운 예외를 `throws` 할 수 없다.
  
```java
class Point {
  int x = 4;
  int y = 5;
  
  String getPosition() {
    return this.x + ", " + this.y;
  }
}

class Point3D extends Point {
  int z = 6;

  // 함수 오버라이딩
  String getPosition() {
    return this.x + ", " + this.y + ", " + this.z;
  }
}
```

#### 어노테이션(annotation)

> 프로그램에게 추가적인 정보를 제공해주면서 기능을 수행하게 하는 메타데이터

1. 컴파일러에게 코드 작성 문법 에러를 체크하도록 정보를 제공
2. 개발툴이 빌드나 배치시 코드를 자동으로 생성할 수 있도록 정보 제공
3. 실행 시 특정 기능을 실행하도록 정보를 제공

장점 : 편리함  
단점 : 생략이 너무 많음 > 가독성이 떨어짐

#### @Override

오버라이딩 코드 작성 문법 에러를 체크하도록 강제하는 어노테이션

```java
class Point3D extends Point {
  int z = 6;

  @Override
  String getPosition() {
    return this.x + ", " + this.y + ", " + this.z;
  }
}
```

<hr>

### 2. super

#### 부모 생성자

자바에서는 자식 객체를 생성하면 부모 객체가 먼저 생성됨  
모든 객체는 생성자를 호출해야하듯 부모 객체도 생성자 호출이 필요함

이러한 역할을 하는 것이 `super()` 이다.  
컴파일시 자식 생성자에 자동 추가 되어 부모 클래스의 기본생성자를 호출한다.

```java
public ChildClass(...) {
  super();  // 컴파일시 자동 추가
  ...
}
```

만약 부모 클래스에 기본 생성자가 없다면 에러가 컴파일 에러가 발생한다.

```java
class Parent {
  String pname;
  Parent(String pname) {
    this.pname = pname;
  }
}

class Child {
  String cname;
  Child() { 
    this.cname = cname;  // 컴파일 에러 - 부모 기본 생성자가 없음
  }
}
```

이런 경우는 `super(매개값...)` 코드를 따로 넣어줘야한다.

```java
class Child {
  String cname;
  Child() { 
    super("부모님");
    this.cname = cname;
  }
}
```

#### 부모 메소드 호출

`super`와 `.`메서드를 통해서 부모의 자원에도 접근 가능하다.  

오버라이딩된 부모 메서드에 접근할 때 유용하게 쓰인다.

```java
class Base {
  String basename;
  Base() {
    System.out.println("부모 클래스 기본 생성자");
  }
  
  Base(String basename) {
    this.basename = basename;
    System.out.println("this.basename : " + this.basename);
  }
  
  void method() {
    System.out.println("부모 method");
  }
}

class Derived extends Base {
  String dname;
  Derived() {
    System.out.println("자식 클래스 기본 생성자");
  }
  Derived(String dname) {
    super(dname);
    this.dname = dname;
    System.out.println("this.dname : " + this.dname);
  }
  
  @Override
  void method() {
    System.out.println("부모함수 재정의");
  }
  
  void parentMethod() {
    super.method();
  }
}
```

<hr>

### 3. protected

#### protected 필드

> 같은 패키지 내에선 `default`, 다른 패키지에선 자식 클래스만 접근 혀용

- 상속과 관련있는 접근 제한자로 `public`과 `default` 중간쯤에 위치
- 필드, 생성자, 메소드 선언에 사용될 수 있다.

```java
package package1;

public class Pclass {
  public int i;
  private int p;
  int d;  // default
  protected int k;
}
```

```java
import package1.Pclass;
class Child extends Pclass{
  // 다른 패키지이지만 상속 관계이므로 k 사용 가능
  protected void method() {
    this.k = 100;
    System.out.println(this.k);
  }
}
```

#### protected 메서드

`protected`로 선언된 메서드는 상속관계에서 재정의하라는 의미를 지닌다.

여기서 오버라이딩할 때 `protected`보다 같거나 넓은 접근제한자를 사용하여야한다.

```java
package package1;

public class Pclass {
  public int i;
  private int p;
  int d;  // default
  protected int k;
  // 재정의하길 원하는 함수
  protected void method() {}
}
```

```java
class Child2 extends Pclass{
  
  @Override
  protected void method() {
    this.k = 100;
    System.out.println(this.k);
  }

}
```



<hr><br>

## Ⅲ. final

### 1. fianl의 역할

상수를 선언할 때 사용

초기값 설정 이후 값을 변경할 수 없음

이 때 변수는 대문자로 작성하는 것이 대표적

```java
final double PI = 3.14;
```

<hr>

### 2. final 필드

`final`로 선언된 필드는 초기화를 보장해주어야한다.  
보장해주지 않으면 에러가 발생한다.

```java
class Vcard {
  final String KIND;  // 컴파일 에러
  final int NUM;  // 컴파일 에러
}
```

즉, 생성자에서 초기화를 보장해주거나 선언과 할당을 동시 진행해야한다.
```java
// 선언과 함께 할당
class Vcard {
  final String KIND = "heart";  // 컴파일 에러
  final int NUM = 1;  // 컴파일 에러
}

// 생성자를 통해 초기화
class Vcard2 {
	final String KIND;
	final int NUM;
	
	Vcard2(String KIND, int NUM) {
		this.KIND = KIND;
		this.NUM = NUM;
	}
}
```

<hr>

### static final

> 모든 영역에서 고정된 값으로 사용하는 상수

- 클래스 자체에 존재하는 단 하나의 상수
- 클래스의 선언과 동시에 반드시 초기화가 필요
- 메모리의 클래스 영역에 존재하여 모든 객체들이 공유

```java
static final double PI = 3.14;
```

<hr>

### final 클래스

클래스를 선언할 때 `final`이 붙으면 최종 클래스라는 말이다.  
즉, 이 클래스는 부모 클래스가 될 수 없다.

```java
final class Parent {
  ...
}

class Child extends Parent{  // 컴파일 에러
  ...
}
```

<hr>

### final 메소드
메소드를 선언할 때 `final`이 붙으면 최종 메소드라는 말이다.  
죽, 자식 클래스에서 이 함수를 오버라이딩 할 수 없다.

```java
class Parent {
  final void method() {}
}

class Child extends Parent {
  @Override
  void method() {}  // 컴파일 에러
}
```

<hr><br>

## Ⅳ. 다형성

### 1. 타입 변환

#### 자동 타입 변환(자식 > 부모)

> 자동으로 타입 변환이 일어나는 것을 말함

`부모타입 변수 = 자식타입객체`의 상황에서 일어난다

```java
class Tv {
  boolean power;
  int ch;
}

class CapTv extends Tv {
  String text;
}

public class ExTv {

  public static void main(String[] args) {
    CapTv ct = new CapTv();
    Tv tv = ct;  // 자동 타입 변환 발생

  }

}
```

이 때 `ct`와 `tv`는 같은 주소값 가지고 같은 객체를 참조한다.

하지만 `tv`는 `text` 필드는 참조하지 못 한다.  
왜냐하면 부모 클래스에서 정의된 필드와 메서드만 참조 가능하기 때문이다.

대신, 메서드는 자식 클래스에서 재정의한 메서드를 참조한다.

#### 강제 타입 변환(부모 > 객체)

자식 타입을 부모 타입으로 싶다면 다음과 같이 해야한다.

`자식타입 변수 = (자식 타입) 부모타입객체;`

여기서 `(자식 타입)`을 캐스팅 연산자라고 한다.

이러한 변환은 무작정 사용할 수 있는 게 아니고
자식타입을 부모타입으로 변환한 후에만 사용 가능하다.

```java
Parent parent = new Child(); 
Child child = (Child) parent; 
```

<!-- ### 2. 필드 다형성

### 3. 매개변수 다형성 -->

