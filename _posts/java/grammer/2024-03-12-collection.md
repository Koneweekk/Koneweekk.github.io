---
title: JAVA - 컬렉션(Collection)
date: 2024-03-12 14:40:00 +09:00
categories: [Java, 문법]
tags:
  [
    프로그래밍 언어,
    Java,
    컬렉션,
    자료구조,
  ]
---

## Ⅰ. 컬렉션 프레임워크

### 1. 컬렉션 프레임워크란?

> 다수의 데이터를 다루는 표준화된 인터페이스를 구현하고 있는 클래스 집합

자바에서 자주 쓰는 자료구조를 효율적으로 사용할 수 있는  
인터페이스와 클래스를 `java.util` 패키지에 포함시켜놓음

<hr>

### 2. 컬렉션 프레임워크의 구성

![컬렉션 프레임워크란](https://mblogthumb-phinf.pstatic.net/20130501_226/javaking75_13674098573182SMCi_PNG/2013-01-15_154740.png?type=w420)

`List`와 `Set`은 `Collection`이란 공통 인터페이스 상속
- 사용 방법에 공통점이 많기 때문에

`Map` : 키와 값의 쌍으로 관리하는 자료 구조
- `Collection`과 사용 방법이 달라 별개의 인터페이스로 관리
  
<hr>

### 3. 구현 클래스의 종류

#### List

> 순서 유지 및 저장, 중복 저장 가능

- `ArrayList`, `Vector`, `LinkedList`

#### Set

> 순서를 유지하지 않고 저장, 중복 저장 불가

- `HashSet`, `TreeSet`

### Map

> 키와 값의 쌍, 키는 중복 저장 불가

- `HashMap`, `Hashtable`, `TreeMap`, `Properties`

<hr><br>


## Ⅱ. List 컬렉션

### 1. List의 특징

#### 동적 배열
- 정적 배열인 `Array`와 달리 크기의 축소와 확대가 가능
- 내부적으로는 정적 배열이 자동 생성 관리되는 것
  
```java
Vector<String> v= new Vector<String>();

System.out.println(v.capacity());  // 10

for (int i = 0 ; i < 11; i++) {
  v.add("A");
}

// 할당된 공간이 자동으로 추가된
System.out.println(v.capacity());  // 20
```

#### 순서 유지와 중복 가능

순서를 유지(내부적으로는 `Array`를 통해 데이터 관리)
- 중복값은 index로 구분

```java
Vector<String> v= new Vector<String>();

for (int i = 0 ; i < 11; i++) {
  v.add("A");
}

for (int i = 0 ; i < 11; i++) {
  System.out.println(v.get(i));
}
```

비순차적인 데이트 추가, 삭제가 가능은 하나 비효율적이다.
- 순차적 데이터 추가, 삭제에 유리

<hr>

### 2. 타입 강제

- 배열 요소의 타입을 지정하여 에러를 방지할 수 있다.

```java
Vector<String> v = new Vector<String>();
v.add("hello");
v.add("world");
v.add(2);  // 컴파일 에러
```

<hr>

### 3. 구현 클래스

#### Vector

동기화가 보장된 메소드로 구성되어 있다.
- 멀티 스레드가 동시에 `Vector` 메서드 수행 불가



#### ArrayList

`Vector`와 달리 동기화가 보장되어있지 않다.
- 멀티 스레드가 동시에 `ArrayList` 메서드 수행 가능

<hr>

### 4. 관련 메서드

#### `add`

배열 요소 추가 함수로 `add`를 사용
- 매개변수를 넣지 않으면 가장 뒤에 추가됨

```java
ArrayList alist = new ArrayList();

alist.add(100);
alist.add(200);
alist.add(300);
```

특정 위치에 요소를 추가하는 것도 가능
- 해당 위치 뒤 데이터들은 뒤로 밀려남
- 뒤로 밀려나는 데이터간 자리 바꿈이 일어나며 비효율 발생

```java
ArrayList alist = new ArrayList();

alist.add(100);
alist.add(200);

alist.add(0, 300);

System.out.println(alist.toString());  // [300, 100, 200]
```

#### `get`

해당 인덱스에 해당하는 배열의 요소를 반환

```java
for (int i = 0; i < alist.size(); i++) {
  System.out.println(alist.get(i));
}
// 100
// 200
// 300
```

#### `contains`

배열이 매개변수를 포함하는지 확인 하는 함수

```java
ArrayList alist = new ArrayList();

alist.add(100);
alist.add(200);

System.out.println(alist.contains(200));  // true
```



#### `isEmpty`

현재 배열이 비어있는지 반환

```java
ArrayList alist = new ArrayList();

alist.add(100);

System.out.println(alist.isEmpty());  // false
```

#### `size`

현재 배열에 들어있는 데이터의 개수 반환

```java
ArrayList alist = new ArrayList();

alist.add(100);
alist.add(200);

System.out.println(alist.size());  // 2
```

#### `clear`

현재 배열에 들어있는 데이터 모두 삭제

```java
ArrayList alist = new ArrayList();
alist.add(100);
alist.add(200);
System.out.println(alist.size());  // 2

alist.clear();
System.out.println(alist.size());  // 0
```

#### `remove`

해당 인덱스의 데이터 삭제
- 그 뒤 데이터들이 한 칸씩 앞으로 당겨짐

```java
ArrayList alist = new ArrayList();

alist.add(100);
alist.add(200);

alist.remove(0);
System.out.println(alist.toString());  // [ 200 ]
```

<hr><br>

## 참고

- [https://m.blog.naver.com/javaking75/140188296229](https://m.blog.naver.com/javaking75/140188296229)