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


<hr>

### 4. 재너릭

> 데이터의 타입(data type)을 일반화한다(generalize)는 것을 의미

제너릭을 설정한 클래스는 대략적으로 아래와 형식이 비슷하다
- 사용자가 입력한 타입(`T`)을 활용 가능

```java
class MyGeneric <T> {
  T obj;

  void add(T obj) {
    this.obj = obj;
  }

  T get() {
    return this.obj;
  }
}
```

컬렉션에도 제너릭을 사용하여 타입 안정성을 챙길 수 있다.

```java
Vector<String> v = new Vector<String>();
v.add("hello");
v.add("world");
v.add(2);  // 컴파일 에러
```

타입을 미리 지정해둠으로써 편리한 사용이 가능하다.
- 형변환 검증과 실행 등에 들이는 노력을 줄일 수 있다.

```java
List<Emp> empList2 = new ArrayList<Emp>();

empList2.add(new Emp(1003, "C", "사원"));
empList2.add(new Emp(1004, "D", "사원"));
empList2.add(new Emp(1005, "E", "사원"));

for (int i = 0; i < empList2.size(); i++) {
  Emp e = empList2.get(i);
  System.out.println(e.getEmpno());
}
```

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

### 2. 구현 클래스

#### Vector

동기화가 보장된 메소드로 구성되어 있다.
- 멀티 스레드가 동시에 `Vector` 메서드 수행 불가



#### ArrayList

`Vector`와 달리 동기화가 보장되어있지 않다.
- 멀티 스레드가 동시에 `ArrayList` 메서드 수행 가능

<hr>

### 3. 관련 메서드

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

#### `subList`

지정된 시작 인덱스부터 끝 인덱스까지 분할한 배열을 반환
- 끝 인덱스에 해당하는 요소는 포함되지 않음

```java
List list = new ArrayList();

list.add("가");
list.add("나");
list.add("다");
list.add("라");

System.out.println(list.subList(0, 2));  // [가, 나]
```

<hr> 

### 4. 추가 메서드

#### `Collections.sort()`

배열의 데이터들을 정렬시키기 위해 사용

`Collections` 클래스의 정적 메서드

```java
List list1 = new ArrayList();
list1.add(50);
list1.add(1);
list1.add(7);
list1.add(40);
list1.add(46);
list1.add(3);
list1.add(15);

Collections.sort(list1);
System.out.println(list1);  // [1, 3, 7, 15, 40, 46, 50]
```

#### `Collections.reverse()`

배열의 데이터들을 뒤집어 저장하기 위한 메서드

`Collections` 클래스의 정적 메서드

```java
Collections.reverse(list1);
System.out.println(list1);  // [1, 3, 7, 15, 40, 46, 50]
```

<hr>

### ※ Stack과 Queue

JAVA API는 `Collections` 기반의 다양한 자료구조 클래스를 제공함 

#### `Stack`

> LIFO(Last in First out)의 자료 구조

- `push` : 데이터 추가
- `pop` : 가장 최근 데이터를 삭제하면서 반환

```java
Stack<String> stack = new Stack<String>();
stack.push("A");
stack.push("B");
stack.push("C");

System.out.println(stack.pop());
```

<hr><br>

## Ⅲ. 이터레이터(Iterator)

### 1. Iterator

`Collections`은 다수의 데이터를 다루는 표준화된 방법 제공하기 위해 수많은 인터페이스를 가지고 있다.

그 중 하나가 `Iterator`이다.

나열되는 자료 구조에 순차적으로 접근하기 위해 사용한다.
- `hasNext` : 다음 데이터가 존재하는지 확인
- `next` : 다음 데이터를 반환

```java
List<Integer> list = new ArrayList<Integer>();
list.add(100);
list.add(200);
list.add(300);

Iterator<Integer> it = list.iterator();
while(it.hasNext()) {
  System.out.println(it.next());
}
// 100
// 200
// 300
```
### 2. ListIterator

`Iterator`는 순방향 순회만 가능하다.

역방향 순회를 위해선 `ListIterator`를 사용하여야한다.
- `hasPrevious` : 이전 데이터가 존재하는지 확인
- `previous` : 이전 데이터를 반환

역방향 순회를 위해선 정방향 순회가 우선되어야한다는 점을 주의하자.

```java
ListIterator<Integer> it3 = list.listIterator();
while(it3.hasNext()) {
  System.out.println(it3.next());
}
// 100
// 200
// 300

while(it3.hasPrevious()) {
  System.out.println(it3.previous());
}
// 300
// 200
// 100
```

<hr>

### 3. remove

`Iterator`를 사용하여 데이터를 제거하기 위해선 `remove`란 메서드를 사용한다.

```java
Iterator<Integer> it = list.iterator();

while(it.hasNext()) {
  int num = it.next();
  // 짝수 삭제
  if(num % 2 == 0) {  
    it.remove();
  }
}

System.out.println(list);  // [1, 3]
```

<hr><br>

## 참고

- [https://m.blog.naver.com/javaking75/140188296229](https://m.blog.naver.com/javaking75/140188296229)