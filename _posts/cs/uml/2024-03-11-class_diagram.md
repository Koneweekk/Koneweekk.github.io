---
title: UML - 클래스 다이어그램
date: 2024-03-11 09:40:00 +09:00
categories: [CS, UML]
tags:
  [
    프로그래밍 언어,
    Java,
    uml,
    클래스 다이어그램
  ]
---

## Ⅰ. 클래스 다이어그램이란?

### 1. UML이란

> Unified Modeling Language : 통합 모델링 언어  
> 객체지향 소프트웨어에서 시스템과 산출물을 명세화, 시각화, 문서화

대표적인 UML 다이어그램 중 하나로  
시스템을 구성하고 있는 클래스들 사이 관계를 나타내기 위해 사용한다.

<hr>

### 2. UML에서의 클래스 표현

|클래스 이름|동물|
|---|---|
|속성(필드)|-종<br>-이름<br>-나이|
연산(메서드)|-밥먹기<br>-잠자기|

- 속성과 연산 부분은 생략 가능
- 속성과 연산을 가시화
- 접근제어자는 아래와 같이 표시
  
|접근제어자|표시|
|---|---|
|`public`|+|
|`private`|-|
|`protected`|#|
|`package`|~|

- 설계 단계에서는 추가적인 정보를 기입한다.

|클래스 이름|Animal|
|---|---|
|속성(필드)|-species:String<br>-name:String<br>-age:Integer|
연산(메서드)|+setName(name:Student):void<br>+getName():void|

<hr>

### 3. 클래스간 관계의 종류

|관계|표시|설명|
|---|---|---|
|연관(Association)|실선 or 화살표|필드로 보유하고 있음|
|일반화(generalization)|빈 화살표|상속 관계|
|집합 - 집약(aggregation)|빈 마름모|전체와 부분의 관계, 전체가 없어져도 부분은 존재|
|집합 - 합성(compostion)|찬 마름모전체와 부분의 관계, 전체가 없어지면 부분도 사라짐|
|의존(dependency)|점선 화살표|메서드에서 사용되는 관계|
|실체화(relization)|점선 빈화살표|인터페이스를 실체화한 관계|

<hr><br>

## Ⅱ. 연관 관계(Association)

### 1. 연관 관계란?

한 클래스가 다른 클래스의 필드가 되어 특정한 역할을 하게 됨
- 연관 관계가 명확한 경우 <u>연관 관계 이름</u>을 사용하지 않아도 된다.
- 실제 구현에서 <u>역할 이름</u>은 각 연간 관계의 클래스를 참조할 수 있는 속성명으로 사용 가능하다.

<hr>

### 2. 단방향 관계

![https://images.velog.io/images/zooneon/post/00dcfaa0-e3c8-4850-afcf-e056853773f8/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-05-13%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%209.13.25.png](https://images.velog.io/images/zooneon/post/00dcfaa0-e3c8-4850-afcf-e056853773f8/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-05-13%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%209.13.25.png)

화살표를 사용

하나의 클래스만 상대방 클래스를 사용

<hr>

### 3. 양방향 관계

![연관관계2](https://velog.velcdn.com/images%2Fzooneon%2Fpost%2F7628fc45-bfc0-458b-8206-f5d5e9501ab5%2Fimage.png)

실선으로 연결 관계 표시

두 클래스가 서로를 사용

- 일반적으로 다대다 관계는 양방향 관계로 표시되는 것이 적절
- 하지만 구현이 복잡하기 때문에 <u>연관 클래스</u>를 사용하여 구현한다.
- 
<hr>

### 4. 연관 클래스

연관 관계에서 추가할 속성이나 행동이 있을 때 사용한다.

연관 클래스를 일반 클래스로 변환하여 다대다 관계를 일대다 관계로 변환시킨다.

<hr>

### 5. 다중성 표시

|표기|의미|
|---|---|
|1|엄밀하게 1|
|* or 0..*|0 또는 그 이상|
|1..*|1 또는 그 이상|
|0...1|0 혹은 1|
|1, 2, 6|1혹은 2혹은 6|

<hr><br>

## Ⅲ. 일반화 관계(Generalization)

### 1. 일반화 관계란?

객체 지향의 개념에선 간단히 말해서 상속 관계를 말한다.

빈화살표를 통해 관계를 표시한다.

![https://images.velog.io/images/zooneon/post/00dcfaa0-e3c8-4850-afcf-e056853773f8/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-05-13%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%209.13.25.png](https://images.velog.io/images/zooneon/post/00dcfaa0-e3c8-4850-afcf-e056853773f8/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-05-13%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%209.13.25.png)

<hr>

### 2. 부모 클래스와 자식 클래스

#### 부모 클래스

- 자식 클래스의 공통 속성이나 연산을 제공하는 틀
- 삼각형이 존재하는 쪽
- 추상 클래스로 구현되기도 한다.
- ex) 가전제품
  
#### 자식 클래스

- 부모 클래스를 구체적으로 구현한 클래스
- 삼격형을 내보내는 쪽
- ex) Tv, 세탁기, 냉장고 등등

<hr>

### 3. 추상 클래스

> 추상 메서드를 하나 이상 가지는 클래스  
> 추상 메서드 : 부모 클래스에서 연산부가 빠진 빈껍데기 메서드

일반 클래스와 달리 객체를 만들 수 없다.
- 클래스명 : `<<클래스명>>` 으로 표시
- 메서드명 : _이탤릭체_ 로 표시

<hr><br>

## Ⅳ. 집합 관계 (Aggregation, Compostion)

### 1. 집합 관계란?

> UML에 존재하는 특별 관계로 전체와 부분의 관계를 정확히 명시하고자 사용

전체와 부분의 라이프사이클의 차이에 따라 "집약"과 "합성" 관계로 나뉜다.

<hr>

### 2. 집약 관계 (Aggregation)

- '전체'를 담당하는 클래스에 빈 마름모로 표시
- 전체 객체의 라이프사이클과 부분 객체의 라이프사이클이 다름
  - 전체 객체가 삭제되어도 부분 객체는 삭제되지 않는다.
- 부분 객체를 다른 전체 객체가 참조할 수 있다.

```java
public class Car {
  // 필드
  Engine engine;
  Wheel[] wheels;
  // 생성자
  public Car(Engine engine, Wheel[] wheels) {
    this.engine = engine;
    this.wheels = wheels;
  }
}
```

<hr>

### 3. 합성 관계(Compostion)
- '전체'를 담당하는 클래스에 찬 마름모로 표시
- 전체 객체의 라이프사이클과 부분 객체의 라이프사이클이 같음
  - 전체 객체가 삭제되어도 부분 객체는 삭제된다.
- 부분 객체를 다른 전체 객체와 굥유할 수 없다.

```java
public class Car {
  // 필드
  Engine engine;
  Wheel[] wheels;
  // 생성자
  public Car() {
    this.engine = new Engine();
    this.wheels = new Wheel[];
  }
}
```

<hr><br>

## Ⅴ. 의존 관계 (dependency)

### 1. 의존 관계란?

> 한 클래스가 다른 클래스를 사용할 때 사용

- 메서드의 매개 변수로 사용될 때
- 메서드 내부 지역 객체로 사용될 때

맴버 필드로 다른 클래스를 사용하는 것은 "연관 관계"이다.

<hr>

### 2. 의존 관계의 특징

메서드가 종료되면 관계가 종료되기 때문에 관계 지속 기간이 짧다.

점선으로 관계를 표시한다.

![의존 관계](https://velog.velcdn.com/images%2Fzooneon%2Fpost%2F31006d19-0106-4b70-a573-c1ade8a142d0%2Fimage.png)

<hr><br>

## Ⅵ. 실체화 관계(Realization)

### 1. 인터페이스 (Interface)

> 구현할 클래스가 가져야 하는 규약, 혹은 책임

클래스가 가질 속성 혹은 메서드를 구현은 하지 않고,  
선언만 해놓은 상태를 가진다.

객체 지향 개념에선 이 관계를 "Can do this"라고 한다.

<hr>

### 2. 실체화 관계의 표현

- 인터페이스 명 : `<<인터페이스명>>`으로 표시
- 빈삼각형과 점선을 통해 관계성 표시

![실체화 관계](https://velog.velcdn.com/images%2Fzooneon%2Fpost%2Fb852ae3b-ac03-4fc7-80c0-1d52c05f0879%2Fimage.png)

<hr><br>

## 참고
- [https://gmlwjd9405.github.io/2018/07/04/class-diagram.html](https://gmlwjd9405.github.io/2018/07/04/class-diagram.html)
- [https://devjaewoo.tistory.com/14](https://devjaewoo.tistory.com/14)
- [https://velog.io/@zooneon/%ED%81%B4%EB%9E%98%EC%8A%A4-%EB%8B%A4%EC%9D%B4%EC%96%B4%EA%B7%B8%EB%9E%A8](https://velog.io/@zooneon/%ED%81%B4%EB%9E%98%EC%8A%A4-%EB%8B%A4%EC%9D%B4%EC%96%B4%EA%B7%B8%EB%9E%A8)