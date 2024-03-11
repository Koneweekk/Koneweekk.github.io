---
title: JAVA - 인터페이스(Interface)
date: 2024-03-11 16:00:00 +09:00
categories: [Java, 문법]
tags:
  [
    프로그래밍 언어,
    Java,
    추상클래스,
    인터페이스
  ]
---

## Ⅰ. 추상 클래스

### 1. 추상 클래스란?

> 미완성된 클래스

- 완성된 코드 + 미완성된 코드로 이루어짐
- 미완성 함수가 존재 : 실행 블럭이 업음
  - ex) `abstract void run()`
  - 스스로 객체 생성 불가
- 상속받은 클래스는 반드시 미완성 요소를 구현해야한다.
  - 즉, 상속이 기본인 클래스

<hr>

### 2. abstract

`abstract`란 키워드로 추상 클래스와 메서드를 선언

이 클래스를 상속받는 클래스는 추상 메서드를 반드시 구현해야 한다.

```java
abstract class Car {
	int pos;
	void run() {
		pos++;
	}
	
	abstract void stop();
}

class Toyota extends Car {
	@Override
	void stop() {
		this.pos = 0;
		System.out.println("stop : " + this.pos);
	}
}

class Bentz extends Car {
	@Override
	void stop() {
		this.pos = 5;
		System.out.println("stop : " + this.pos);
	}
}
```

<hr>

### 3. 다형성의 사용

`abstract`로 선언된 부모 타입 변수에도 자식 객체를 할당할 수 있다.

하지만 부모 타입의 객체를 직접 생성하는 것은 불가능하다.

```java
public class Ex01_abstract_class {

	public static void main(String[] args) {
		Toyota toyota = new Toyota();
		toyota.run();
		toyota.stop();  // 0
		
		Bentz bentz = new Bentz();
		bentz.run();
		bentz.stop();  // 5
		
		Car car = toyota;
		car.stop();  // 0
		
		Car car2 = bentz;
		car.stop();  // 5
		
		Car car3 = new Car();  // 컴파일 에러
		
	}
}
```

<hr><br>

## Ⅱ. 인터페이스(Interface)

### 1. 인터페이스란?

> 약속, 규칙, 규약, 표준을 만드는 것  

구현부가 존재하지 않음(실제로 존재는 하지만 거의 사용 X)
- 인터페이스를 구현할 클래스의 내용을 강제하는 역할

<hr>

### 2. 인터페이스의 특징

#### 추상 클래스와의 공통점

- 추상 자원에 대한 강제적 구현이 목적이다(재정의 강제)
- 필요하다면 인터페이스끼리 상속을 통해서 확장이 가능하다
- 스스로 객체 생성 불가능 (new 연산자 사용불가)
- 인터페이스 타입 변수에 구현 클래스 객체의 주소값 할당 가능


#### 추상 클래스와의 차이점

- 인터페이스는 모든 요소가 미완성이다. (default가 지원되긴함)
- 다중 상속이 불가능한 추상 클래스와 달리 다중 구현이 가능하다.
  - 여러 개의 작은 약속을 통해 큰 약속을 만드는 행위이기 때문
  - 큰 인터페이스는 재사용성이 떨어진다.

<hr>

### 3. 인터페이스 선언

- `interface` : 인터페이스 선언 키워드
- `implements` : 구현 클래스 선언 키워드

```java
interface Ia {
	String print(); 
	void message(String str);
}

class Test extends Object implements Ia {
	@Override
	public String print() {
		...
	}
	@Override
	public void message(String str) {
		...
	}
}
```

다중 구현도 인터페이스에선 사용 가능하다.

```java
interface Ia {
	String print();
	void message(String str);
}

interface Ib {
	int AGE = 10;
	void info();
	String val(String s);
}

class Test implements Ia, Ib {
	@Override
	public void info() {
    ...
	}
	@Override
	public String val(String s) {
	}
	@Override
	public String print() {
    ...
	}
	@Override
	public void message(String str) {
    ...
	}

}
```
<hr>

### 4. 접근제한자의 생략

인터페이스의 상수 표현에 있어 `public static final`은 생략 가능하다.

추상 메소드의 `public abstract` 키워드 또한 생략 가능하다.

- 둘 모두 컴파일러가 자동으로 추가해준다.
- 상수와 추상 메소드는 `public` 이하의 접근제한자를 가지지 못 한다.

```java
interface Ia {
	int AGE = 100;  // public static final 생략
	String GENDER = "남";  // public static final 생략
	String print();  // public abstract 생략
	void message(String str);
}
```

> ※ 추상 메소드 : `{}` 부분을 가지지 않는 메소드. 구현 클래스에서 반드시 재정의 필요

<hr><br>

## Ⅲ. 인터페이스의 다형성

### 1. 자식 객체 주소값 할당

클래스와 같이 정의한 인터페이스 타입 변수에 구현 클래스의 주소를 할당할 수 있다.
- 이 경우 메소드는 구현 클래스에서 재정의한 메소드 로직을 사용

```java
Test2 t = new Test();
Ia ia = t2;  // 구현 클래스 객체의 주소 할당
```

하지만 `new` 생성자를 통한 객체 생성은 불가능하다.

```java
Ia ia = new Ia();  // 컴파일 에러
```

<hr>

### 2. 공통 의미 추가

아무런 공통점이 보이지 않는 클래스들에 공통적인 의미을 부여해줄 수 있다.

```java
class Unit {
  int hitpoint;
  final int MAX_HP;
  Unit2(int hp) {
    this.MAX_HP = hp;
    this.hitpoint = hp;
  }
}

// 수리 가능함
interface Irepairable {}

// 지상 유닛
class GroundUnit extends Unit {
  GroundUnit(int hp) {
    super(hp);
  }
}

// 공중 유닛
class AirUnit extends Unit {
  AirUnit(int hp) {
    super(hp);
  }
}

//건물 - 수리 가능
class CommandCenter implements Irepairable {}

// 탱크 - 수리 가능
class Tank extends GroundUnit implements Irepairable {
  Tank2(int hp) {
    super(hp);
  }
}

// 마린
class Marine extends GroundUnit {
  Marine2(int hp) {
    super(hp);
  }
}

// SCV - 수리가능
class SCV extends GroundUnit implements Irepairable {
  SCV(int hp) {
    super(hp);
  }
}
```

수리 가능한 유닛들은 공통적인 부모 클래스를 지니지 않는다.  

하지만 `Irepairable`이란 인터페이스를 상속함으로써 
수리 가능한 유닛에게만 의미를 부여해줄 수 있다.

<hr>

### 3. 자식 클래스에 따른 로직 분할

인터페이스를 통해 공통 의미를 부여했다해도 클래스 별로 다른 연산을 거쳐야하는 경우가 있다.

그럴 경우 `instanceof`란 함수를 통해 변수에 할당된 클래스를 판단하고,  
그에 맞게 실행문을 상황 별로 달리할 수 있다.

만약 이 때 인터페이스에 없는 필드 혹은 메서드를 써야하는 경우라면  
자식 클래스로 타입을 재변환해줘야한다.

```java
class SCV extends GroundUnit implements Irepairable {
  ...
  
  void repair(Irepairable repairunit) {
    if(repairunit instanceof Unit2) {
      Unit2 unit2 = (Unit2)repairunit;
      unit2.hitpoint = unit2.MAX_HP;
    } else {
      System.out.println("건물을 수리합니다.");
    }
  }

}
```
