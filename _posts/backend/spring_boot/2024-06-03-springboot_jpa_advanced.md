---
title: Spring Boot - JPA 활용
date: 2024-06-03 10:45:00 +09:00
categories: [Back-End, Spring Boot]
tags: [Back-End, Java, Spring Boot, JPA]
---

## Ⅰ. 쿼리 메서드

### 1. 쿼리메서드란?

- 리포지토리는 `JpaRepository`를 상속받는 것만으로도 다양한 CURD 메서드를 제공
- 하지만 기본 메서드들은 식별자 기반으로 생성
- 좀 더 복잡한 쿼리문을 사용하기 위해선 별도의 메서드를 정의해서 사용

---

### 2. 쿼리 메서드의 생성

- 크게 주제와 서술어로 구분
- 주제 : `find...By`, `exist...By` 같은 키워드로 주제를 지정
- 서술어 : `By` 는 서술어의 시작을 알리는 구분자 역할
  - 검색 및 정렬 조건 지정

```java
List<Person> findByLastnameAndEmail(String lastName, String email);
```

---

### 3. 쿼리 메서드의 주제 키워드

#### 조회 키워드 (Find, Read, Get, Query)

- findBy
- readBy
- getBy
- queryBy

```java
List<User> findByLastName(String lastName);
```

- 리턴타입은 엔티티 혹은 엔티티의 배열

#### 존재 확인 키워드

- existsBy

```java
boolean existsByNumber(Long number);
```

- 리턴 타입은 boolean

#### 개수 키워드

- countBy

```java
long countByName(String name);
```

- 조회 쿼리 데이터의 개수를 `long` 타입으로 반환

#### 삭제 키워드

- deleteBy
- removeBy

```java
void deleteByNumber(Long number);
long removeByName(String name);
```

- void 혹은 삭제한 횟수를 `long` 타입으로 반환

#### 결과값 개수 제한

- ...First
- ...Top

```java
List<Product> findFirstByName(String name);

List<Product> findTop10ByName(String name);
```

- 일반적으로 한 번의 동작으로 여러 건을 조회할 때 사용

---

### 3. 쿼리 메서드의 조건자 키워드

#### Is, Equals

- 값이 일치를 조건으로 사용
- 생략되는 경우가 많음

```java
List<User> findByFirstNameIs(String firstName);
List<User> findByFirstNameEquals(String firstName);
```

#### Not

- 값의 불일치를 조건으로 사용
- `Is`는 생략 가능

```java
List<User> findByFirstNameIsNot(String firstName);
```

#### Null, NotNull

- NULL 여부를 조건으로 사용
- `Is`는 생략 가능

```java
List<User> findByFirstNameIsNull();

List<User> findByFirstNameIsNotNull();
```

#### And, Or

- 여러 조건을 묶을 때 사용

```java
List<User> findByLastNameAndFirstName(String lastName, String firstName);

List<User> findByLastNameOrFirstName(String lastName, String firstName);
```

#### GreaterThan, LessThan, Between

- 숫자나 datetime 칼럼을 대상으로 한 비교 연산에 사용
- `Than`이 붙으면 이상/이하, 없으면 초과 미만

```java
List<User> findByAgeGreaterThan(int age);
List<User> findByAgeGreaterThanEqual(int age);

List<User> findByAgeLessThan(int age);
List<User> findByAgeLessThanEqual(int age);

List<User> findByAgeBetween(int startAge, int endAge);
```

#### StartingWith, EndingWith, Containing, Like

- 칼럼값에서 일부 일치 여부를 확인
- SQL의 `%`dhk ehddlfgks durgkf
- `Containing`은 양쪽 끝

```java
List<Entity> findByColumnNameStartingWith(String prefix);

List<Entity> findByColumnNameEndingWith(String suffix);

List<Entity> findByColumnNameContaining(String partialValue);
List<Entity> findByColumnNameContains(String partialValue);

List<Entity> findByColumnNameLike(String pattern);
```

---
<br>

## Ⅱ. 정렬과 페이징 처리

### 1. 정렬 처리

#### 일반 정렬

```java
List<Product> findByNameOrderByNumberAsc(String name);
List<Product> findByNameOrderByNumberDesc(String name);
```

- `OrderBy` 정렬하고자 하는 칼럼과 오름차순/내림차순을 설정
- `Asc` : 오름차순
- `Desc` : 내림차순

#### 다중 정렬

```java
List<Product> findbyNameOrderByPriceAscStockDesc(String name);
```

- 다중 정렬시 `And`는 생략
- 앞쪽부터 차례대로 정렬 후순위 적용

#### 매개변수 활용

```java
List<Product> findbyName(String name, Sort sort);
```

- 키워드를 사용하지 않고 `Sort` 객체를 매개변수로 받아들인 정렬 기준을 사용

```java
productRepository.findByName("펜", Sort.by(Order.asc("pirce"), Order.desc("stock")));
```

- 메서드 호출 코드는 다음과 같다
- 내부 클래스인 `Order`를 활용하여 ㅈ어렬 기준 생성
- 다중 정렬인 경우 `,`를 통해 순서 표시

```java
class ProductRepository {
  ...
  void sortingAndPaging() {
    return productRepository.findByName("펜", getSort());
  }

  private Sort getSort() {
    return Sort.by(Order.asc("pirce"), Order.desc("stock"));
  }
}
```

- 위처럼 별도의 메서드로 분리하여 사용 가능

---

### 2. 페이징 처리

> 페이징 : 페이지를 개수로 나눠 페이지를 구분하는 것

JPA에선 페이징 처리를 위해 Page와 Pageable을 사용

```java
// 메서드 정의
...
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.data.jpa.repository.JpaRepository;

public interface ProductRepository extends JpaRepository<Product, Long>{
  ...

  Page<Product> findByName(String name, Pageable pageable);
}
```

- 리턴 타입으로 `Page`를 설정하고 매개변수에 `Pagebale` 타입 객체 정의
  

```java
// 메서드 호출은 위와 같음
Page<Product> productPage = productRepository.findByName("펜", PageRequest(0, 2));
```

- `PageRequest` 객체를 통해 `Pagebale` 타입 매개변수 전달

```java
System.out.println(productPage);  // Page 1 of 2 ...
System.out.println(productPage.getContent);  // [item1, item2, ...] 
```

- `Page` 객체 그대로는 몇 번째 페이지에 해당하는지를 출력
- `getContent` 메서드 활용시 값이 배열형태로 출력됨


#### `PageRequest`

- 주로 `of` 메서드를 통해 `PageRequest` 객체 생성
- 매개변수에 따라 다양한 형태로 오버로딩

```java
// 기본 페이징
public static PageRequest of(int page, int size)
```

- `page`: 페이지 번호 (0부터 시작)
- `size`: 페이지 크기 (페이지당 항목 수)

```java
// 페이징 및 정렬
public static PageRequest of(int page, int size, Sort sort)
```
- `sort`: 정렬 기준

```java
// 페이징 및 정렬 방향
public static PageRequest of(int page, int size, Sort.Direction direction, String... properties)
```
- `direction` : 정렬 방향 (Sort.Direction.ASC 또는 Sort.Direction.DESC)
- `properties` : 정렬할 속성 목록

---
<br>

## Ⅲ. JPQL

### 1. JPQL이란?

> JPA Query Language의 줄임말로 JPA에서 사용할 수 있는 쿼리

SQL과 유사하지만, 데이터베이스 테이블이 아닌 엔티티 객체를 대상으로 쿼리를 작성

```sql
SELECT e FROM EntityType e WHERE e.property = ?1;
```

- `EntityType` : 엔티티 타입
- `property` : 엔티티 속성

---

### 2. @Query

```java
@Query("SELECT p FROM Product AS p WHERE p.name = ?1")
List<Product> findByName(String name);
```

- `@Query` 어노테이션을 사용해 JPQL 형식의 쿼리문을 작성
- 별칭 작성시 `AS` 생략 가능
- `?1` : 파라미터를 전달받기 위한 인자(첫번째 인자)

```java
@Query("SELECT p FROM Product p WHERE p.name = :name")
List<Product> findByName(@Param("name") String name);
```

- 파라미터의 순서가 바뀌면 오류가 발생할 가능성을 차단
- 코드의 가독성과 유지보수성을 증가시킴

```java
@Query("SELECT p.name, p.price, p.stock FROM Product p WHERE p.name = :name")
List<Object[]> findByName(@Param("name") String name);
```

- 위처럼 원하는 칼럼만 추출 가능
- 이때 `Product` 엔티티는 사용 불가능

---
<br>

## Ⅳ. QueryDSL

### 1. QueryDSL이란

> 정적 타입을 이용해 SQL과 같은 쿼리를 생성할 수 있도록 지원하는 프레임 워크

- 문자열이 아닌 내부적으로 제공하는 플루언트(Fluent API)를 활용

---

### 2. QueryDSL의 장점

- IDE가 제공하는 코드 자동 완성 기능 사용 가능
- 문법적으로 잘못된 쿼리 허용 X
- 고정된 SQL 쿼리를 작성하지 않고 동적 쿼리 작성 가능
- 코드로 작성하기 때문에 가독성과 생산성이 향상됨
- 도메인 타입과 프로퍼티를 안전하게 참조 가능

---

### 3. 프로젝트 설정

build.gradle에 다음과 같이 의존성 추가

```gradle
dependencies {
  ...
	//QueryDSL 추가
	implementation 'com.querydsl:querydsl-jpa:5.0.0:jakarta'
	annotationProcessor "com.querydsl:querydsl-apt:${dependencyManagement.importedProperties['querydsl.version']}:jakarta"
	annotationProcessor "jakarta.annotation:jakarta.annotation-api"
	annotationProcessor "jakarta.persistence:jakarta.persistence-api"
}
```

이후 compile을 수행해 빌드작업을 수행
- `{프로젝트루트폴더}\build\generated\sources\annotationProcessor\java\main\{패키지이름}` 디렉토리 안에 Q로 시작하는 파일이 생성

---

### 4. Query DSL 사용

#### 기본 사용법

QueryDSL에 의해 생성된 Q도메인 클래스를 활용하는 코드는 다음과 같다

```java
public class ProductRepositoryTest {

  @PersistenceContext
  EntityManager entityManager;

  @Test
  void queryDslTest() {
    JPAQuery<Product> query = new JPAQuery(entityManager);
    QProduct qProduct = QProduct.product;

    List<Product> productList = query
                                .from(qProduct)
                                .where(qProduct.name.eq("펜"))
                                .orderBy(qProduct.price.asc())
                                .fetch();
    
    for (Product product : productList) {
      System.out.println(product.toString());
    }
  }
}
```

`JPAQuery`
- `EntityManager`를 활용하여 생성
- 빌더 형식으로 쿼리 작성
- SQL에서 사용되는 키워드로 메서드 구성

`fetch()`
- List 타입 값 반환
- 4.01 이전 버전은 `list()`
- `fetchOne` : 단건 조회
- `fetchFirst` : 다수 조회에서 1건 반환
- `fetchCount` : 조회 결과의 개수 반환
- `QueryResult<T>? fetchResults` : 조회결과 리스트와 개수를 포함한 QueryResults 반환
  
#### JPAQueryFactory

```java
// JPAQueryFactory 사용
@Test
void queryDslTest2() {
  JPAQueryFactory jpaQueryFactory = new JPAQueryFactory(entityManager);
  QProduct qProduct = QProduct.product;

  List<Product> productList = jpaQueryFactory
                              .selectFrom(qProduct)
                              .where(qProduct.name.eq("펜"))
                              .orderBy(qProduct.price.asc())
                              .fetch();
  
  for (Product product : productList) {
    System.out.println(product.toString());
  }
}

// 단일 컬럼 조회
@Test
void queryDslTest3() {
  JPAQueryFactory jpaQueryFactory = new JPAQueryFactory(entityManager);
  QProduct qProduct = QProduct.product;

  List<String> productList = jpaQueryFactory
                              .select(qProduct.name)
                              .from(qProduct)
                              .where(qProduct.name.eq("펜"))
                              .orderBy(qProduct.price.asc())
                              .fetch();
  
  for (String product : productList) {
    System.out.println(product.toString());
  }
}

// 복수 컬럼 조회
@Test
void queryDslTest4() {
  JPAQueryFactory jpaQueryFactory = new JPAQueryFactory(entityManager);
  QProduct qProduct = QProduct.product;

  List<Tuple> productList = jpaQueryFactory
                              .select(qProduct.name, qProduct.price)
                              .from(qProduct)
                              .where(qProduct.name.eq("펜"))
                              .orderBy(qProduct.price.asc())
                              .fetch();
  
  for (String product : productList) {
    System.out.println(product.toString());
  }
}
```

- `JPAQuery`와 달리 `select` 절부터 작성 가능
- 일부 칼럼만 조회하고 싶다면 `select`와 `from` 분리
- 조회 칼럼이 여러개일 경우 쉼표로 구분, 타입은 `List<Tuple>`

---
<br>

## Ⅴ. JPA Auditing

### 1. JPA Auditing이란?

> 엔티티에 공통적으로 들어가는 필드를 자동으로 넣어주는 기능 제공

- 엔티티에 공통적으로 들어가는 필드 : 생성 주체, 생성일자, 변경주체, 변경일자
- 이러한 필들은 매번 엔티티를 생성하거나 변경할 대마다 값을 주입해야하는 번거로움이 존재

---

### 2. JPA Auditing 활성화

#### 방법1

```java
@SpringBootApplication
@EnableJpaAuditing
public class JpaAdvancedApplication {

	public static void main(String[] args) {
		SpringApplication.run(JpaAdvancedApplication.class, args);
	}

}
```

- `main` 메서드가 있는 클래스에 `@EnableJpaAuditing` 추가
- 테스트 코드 작성하여 애플리케이션 테스트하는 상황에서 오류 발생 가능

#### 방법2

```java
@Configuration
@EnableJpaAuditing
public class JpaAuditingConfiguration {
  ...
}
```

- 별도의 `Configuration` 클래스를 생성해 애플리케이션 클래스와 분리 가능
- 1번 방법과는 병행 불가능

---

### 3. 베이스 엔티티

- 공통으로 들어가는 칼럼을 하나의 클래스로 빼는 작업을 수행해야함
- 반드시는 아니지만 자주 활용되는 기법

```java
@Getter
@Setter
@ToString
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
public class BaseEntity {

  @CreatedDate
  @Column(updatable = false)
  private LocalDateTime createdAt;

  @LastModifiedDate
  private LocalDateTime updatedAt;
}
```

- `@MappedSuperclass` : JPA의 엔티티 클래스가 상속받을 경우 자식 클래스에게 매핑 정보 전달
- `@EntityListeners` : 엔티티를 데이터베이스에 적용하기 전후로 콜백을 요청할 수 있게함
- `AuditingEntityListener` : 엔티티의 Auditing 정보를 주입하는 JPA 엔티티 리스너 클래스
- `@CreatedDate` : 데이터 생성 날짜 자동 주입
- `@LastModifiedDate` : 데이터 수정 날짜 자동 주입
- `@CreatedBy` : 엔티티 생성 주체 주입
- `@updatedBy` : 엔티티 변경 주체 주입

```java
@Entity
@Setter
@Getter
@NoArgsConstructor
@AllArgsConstructor
@Builder
@ToString(callSuper = true)
@EqualsAndHashCode(callSuper = true)
@Table(name="product")
public class Product extends BaseEntity {
  
  @Id
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  private Long number;

  @Column(nullable = false)
  private String name;

  @Column(nullable = false)
  private int price;

  @Column(nullable = false)
  private int stock;
}
```

- `BaseEntity`를 상속함으로써 공통 필드가 추가됨
- `callSuper` : 부모 클래스의 필드를 포함하는 역할