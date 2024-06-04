---
title: Spring Boot - JPA 연관관계 매핑
date: 2024-06-04 09:00:00 +09:00
categories: [Back-End, Spring Boot]
tags: [Back-End, Java, Spring Boot, JPA]
---

## Ⅰ. JPA와 연관 관계

### 1. 연관 관계의 종류

- One To One : 일대일(1:1)
- One To Many : 일대다(1:N)
- Many To One : 다대일(N:1)
- Many To Many : 다대다(N:N)

---

### 2. JPA에서의 연관 관계
  
JPA : 엔티티 간 참조 방향을 설정 가능
- DB 관계와 일치시키기 위해 양방향으로 설정하기도 하지만 단반향 관계만을 해결되는 경우가 많음
- 단방향 : 한 쪽의 엔티티만 다른 한쪽을 참조하는 형식
- 양방향 : 두 엔티티가 서로 참조하는 형식

연관관계가 설정되면 한 테이블에서 다른 테이블의 기본값을 외래키로 갖게됨
- 이런 관계에서는 주인이라는 개념이 사용됨
- 일반적으로 외래키를 가진 테이블이 그 관계의 주인이 됨
- 주인은 외래키를 사용할 수 있으나 상대 엔티티는 읽는 작업만 수행

---
<br>

## Ⅱ. 일대일 매핑

### 1. 일대일 단방향 관계 설정

관계를 설정할 데이터의 관계는 아래 사진과 같다.

![일대일 단방향](/assets/img/post/backend/spring-boot/2024-06-04-springboot_jpa_mapping/01.png)

```java
// Product 엔티티
@Entity
@Getter
@Setter
@NoArgsConstructor
@ToString(callSuper = true)
@EqualsAndHashCode(callSuper = true)
@Table(name = "product")
public class Product extends BaseEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long number;

    @Column(nullable = false)
    private String name;

    @Column(nullable = false)
    private Integer price;

    @Column(nullable = false)
    private Integer stock;
}
```

아래 코드는 `Product` 엔티티와 관계를 맺어줄 `ProductDetail` 엔티티이다.

```java
@Entity
@Table(name = "product_detail")
@Getter
@Setter
@NoArgsConstructor
@ToString(callSuper = true)
@EqualsAndHashCode(callSuper = true)
public class ProductDetail extends BaseEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String description;

    @OneToOne
    @JoinColumn(name = "product_number")
    private Product product;

}
```

#### `@OneToOne`
- 다른 엔티티를 필드로 정의했을 때 일대일 연관관계로 매핑하기 위해 사용

`@JoinColumn`
- 매핑할 위래키를 설정
- 기본값이 설정돼 있자만 의도한 이름이 들어가지 않을 수 있어 `name`을 따로 지정해주는 게 좋음
- 이 어노테이션을 선언하지 않으면 매핑용 중간 테이블이 생기게 됨
- `name` : 매핑할 외래키의 이름 설정
- `referenedColumnName` : 외래키가 참조할 상대 테이블의 칼럼명을 지정
- `foreignKey` : 외래키를 생성하면서 지정할 제약조건 설정

#### 기능 테스트

`ProductDetailRepository`를 생성하여 생성 및 조회 기능 테스트해보자

```java
public interface ProductDetailRepository extends JpaRepository<ProductDetail, Long> {

}
```

그 후 테스트 코드를 통해 기능을 테스트

```java
@SpringBootTest
class ProductDetailRepositoryTest {

    @Autowired
    ProductDetailRepository productDetailRepository;

    @Autowired
    ProductRepository productRepository;

    @Test
    public void saveAndReadTest1() {
        Product product = new Product();
        product.setName("스프링 부트 JPA");
        product.setPrice(5000);
        product.setStock(500);

        productRepository.save(product);

        ProductDetail productDetail = new ProductDetail();
        productDetail.setProduct(product);
        productDetail.setDescription("스프링 부트와 JPA를 함께 볼 수 있는 책");

        productDetailRepository.save(productDetail);

        // 생성한 데이터 조회
        System.out.println("savedProduct : " + productDetailRepository.findById(
                productDetail.getId()).get().getProduct());

        System.out.println("savedProductDetail : " + productDetailRepository.findById(
                productDetail.getId()).get());
    }
}
```

`productDetail.setProduct(product)`
- `ProductDetail`객체에 매핑될 `Product` 객체를 지정

`productDetailRepository.findById(productDetail.getId()).get().getProduct()`
- `ProductDetail` 객체를 조회한 후 연관 매핑된 `Product`를 조회

---

### 2. @OneToOne

`@OneToOne`의 내부 코드는 다음과 같다.

```java
public @interface OneToOne {

    Class targetEntity() default void.class;

    CascadeType[] cascade() default {};

    FetchType fetch() default EAGER;

    boolean optional() default true;

    String mappedBy() default "";

    boolean orphanRemoval() default false;
}
```

`fetch()`
- 기본 fetch 전략으로 `EAGER` 채택
- 즉, 즉시 로딩 전략(엔티티 조회시 연관된 엔티티도 함께 조회)이 채택

`optional()`
- 기본값 `true` : 매핑되는 값이 `nullabe`이라는 뜻
- `@OneToOne(optional = false)` : 매핑되는 값을 반드시 넣어야함

---

### 3. 일대일 양방향 매핑

> 양방향 매핑 = 단방향으로 서로를 매핑

양방향 매핑을 위해 `Product` 엔티티의 코드를 다음과 같이 변경

```java
@Entity
@Getter
@Setter
@NoArgsConstructor
@ToString(callSuper = true)
@EqualsAndHashCode(callSuper = true)
@Table(name = "product")
public class Product extends BaseEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long number;

    @Column(nullable = false)
    private String name;

    @Column(nullable = false)
    private Integer price;

    @Column(nullable = false)
    private Integer stock;

    // 매핑 필드 추가
    @OneToOne
    private ProductDetail productDetail;
}
```

이런 식으로 양방향 관계를 맺어주면 내부적으로는 join 키워드가 두번이나 들어감
- 두 테이블이 양쪽 모두 외래키를 가지고 있기 때문
- 이럴 경우 양방향 매핑은 하되 한쪽에게만 외래키를 주는 것이 좋음

```java
@OneToOne(mappedBy = "product")
private ProductDetail productDetail;
```

`mappedBy`
- 어떤 객체가 주인인지 표시하는 속성
- 연견관계를 갖고 있는 상대 엔티티에 있는 연관관계 필드의 이름을 값으로 가짐
- 위 코드의 경우 `ProductDetail`가 `Product`의 주인 엔티티가 됨

```java
@OneToOne(mappedBy = "product")
@ToString.Exclude
private ProductDetail productDetail;
```

`@ToString.Exclude`
- 양방향관계의 경우 `@ToString` 사용시 순환참조 에러가 발생하여 해결하기 위해 사용
- 필요한 경우가 아니면 그냥 단방향 연관관계가 좋음

---
<br>

## Ⅲ. 일대다, 다대일 매핑

하나의 공급업체가 다수의 상품을 공급한다.  
- 상품 테이블 입장에선 다대일
- 공급업체 테이블 입자에선 일대다

![일대일 단방향](/assets/img/post/backend/spring-boot/2024-06-04-springboot_jpa_mapping/01.png)

### 1. 다대일 단방향 매핑

우선 공급업체 테이블에 매칭되는 엔티티 클래스를 정의

```java
@Entity
@Getter
@Setter
@NoArgsConstructor
@ToString(callSuper = true)
@EqualsAndHashCode(callSuper = true)
@Table(name = "provider")
public class Provider extends BaseEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name

}
```

그 후 상품 엔티티에서 공급업체의 번호를 받기 위해 `Product` 엔티티 클래스에 다음 코드를 추가

```java
@ManyToOne
@JoinColumn(name = "provider_id")
private Provider provider;
```

외래키를 갖는 쪽이 주인 역할을 수행
- 위 경우 상품 엔티티가 공급업체 엔티티의 주인

#### 기능 테스트

`ProductDetailRepository`를 생성하여 생성 및 조회 기능 테스트해보자

```java
public interface ProviderRepository extends JpaRepository<Provider, Long> {

}
```

그 후 테스트 코드를 통해 기능을 테스트

```java
@SpringBootTest
class ProviderRepositoryTest {

    @Autowired
    ProductRepository productRepository;

    @Autowired
    ProviderRepository providerRepository;

    @Test
    void relationshipTest1() {
        // 테스트 데이터 생성
        Provider provider = new Provider();
        provider.setName("ㅇㅇ물산");

        providerRepository.save(provider);

        Product product = new Product();
        product.setName("가위");
        product.setPrice(5000);
        product.setStock(500);
        product.setProvider(provider);

        productRepository.save(product);

        // 테스트
        System.out.println(
                "product : " + productRepository.findById(1L).orElseThrow(RuntimeException::new));

        System.out.println("provider : " + productRepository.findById(1L).orElseThrow(RuntimeException::new).getProvider());
    }

}
```

데이터 저장
- 쿼리로 데이터를 저장할 때는 `provider_id` 값만 들어감
- `@JoinColumn`에 설정한 이름을 기반으로 자동으로 값 선정 후 추가

데이터 조회
- 단방향 관계이기 때문에 `productRepository` 만으로도 `Provider` 객체 조회 가능

---

### 2. 다대일 양방향 매핑

공급업체를 통해서 등록된 상품을 조회하기 위해선 양방향 매핑 필요

`Provider` 엔티티 클래스에 다음과 같은 코드 추가

```java
@OneToMany(mappedBy = "provider", fetch=FetchType.EAGER)
@ToString.Exclude
private List<Product> productList = new ArrayList<>();
```

`List<Product>`
- 일대다 연관관계의 경우 여러 상품 엔티티가 포함될 수 있어 컬렉션 형식으로 필드 생성

`@OneToMany`
- 이 어노테이션이 붙은 쪽에서 `@JoinColumn`을 사용하면 상대 엔티티에 필드를 생성

`@ToString.Exclude`
- 롬복 순환 참조 에러 발생 방지

`fetch=FetchType.EAGER`
- `@OneToMany`의 기본 전략은 Lazy 전략
- 즉시 로딩 전략으로 변경

위 코드를 추가해도 칼럼은 변경되지 않는다.
- `mappedBy`로 선정된 필드는 칼럼에 적용되지 않음
- 양쪽에서 연관관계 설정시 RDBMS 형식처럼 사용하기 위해 한 쪽으로 외래키 관리 위임

#### 기능 테스트

공급업체 엔티티를 통해 제품 엔티티의 값을 가져올 수 있는지 테스트

```java
@Test
void relationshipTest() {
    // 테스트 데이터 생성
    Provider provider = new Provider();
    provider.setName("ㅇㅇ상사");

    providerRepository.save(provider);

    Product product1 = new Product();
    product1.setName("펜");
    product1.setPrice(2000);
    product1.setStock(100);
    product1.setProvider(provider);

    Product product2 = new Product();
    product2.setName("가방");
    product2.setPrice(20000);
    product2.setStock(200);
    product2.setProvider(provider);

    Product product3 = new Product();
    product3.setName("노트");
    product3.setPrice(3000);
    product3.setStock(1000);
    product3.setProvider(provider);

    productRepository.save(product1);
    productRepository.save(product2);
    productRepository.save(product3);

    // 테스트 실시
    List<Product> products = providerRepository.findById(provider.getId()).get()
            .getProductList();

    for (Product product : products) {
        System.out.println(product);
    }

}
```

`Provider`는 연관관계 주인이 아니기 때문에 외래키 관리 불가
- `Provider` 객체를 생성하여 각 `Product` 객체에 `Provider` 등록

---

### 3. 일대다 단방향 매핑

![일대다 단방향](/assets/img/post/backend/spring-boot/2024-06-04-springboot_jpa_mapping/03.png)

위 관계를 나타내기 위해 상품 분류 엔티티를 생성

```java
@Entity
@Getter
@Setter
@NoArgsConstructor
@ToString
@EqualsAndHashCode
@Table(name = "category")
public class Category {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(unique = true)
    private String code;

    private String name;

    @OneToMany(fetch = FetchType.EAGER)
    @JoinColumn(name = "category_id")
    private List<Product> products = new ArrayList<>();

}
```

- 상품 테이블에 외래키 추가
- `@OneToMany`와 `@JoinColumn`을 사용하면 상대 엔티티에 별도의 설정 없이 일대다 단방향 매핑
- 이 방식은 다대일 구조와 다르게 외래키 설정을 위해 다른 테이블에 대한 update 쿼리 발생
- 이런 문제점을 방지하기 위해 다대일 구조를 사용하는 것이 좋음

---
<br>

## Ⅳ. 다대다 매핑

- 한 종류의 제품이 여러 생산업체를 통해 생산 가능
- 한 생산업체는 여러 종류의 제품을 생산 가능

![다대다 매핑](/assets/img/post/backend/spring-boot/2024-06-04-springboot_jpa_mapping/04.png)

다대다 연관관계에선 각 엔티티가 서로를 리스트로 가지는 구조가 됨
- 교차 엔티티라고 부르는 중간 테이블을 생성
- 예상치 못한 쿼리가 발생할 수 있음

### 1. 다대다 단방향 매핑

우선 생산업체 엔티티를 생성

```java
@Entity
@Getter
@Setter
@NoArgsConstructor
@ToString(callSuper = true)
@EqualsAndHashCode(callSuper = true)
@Table(name = "producer")
public class Producer extends BaseEntity{

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String code;

    private String name;

    @ManyToMany
    @ToString.Exclude
    private List<Product> products = new ArrayList<>();

    public void addProduct(Product product){
        products.add(product);
    }

}
```

`@ManyToMany`
- `@JoinColumn` 불필요
- 리스트로 필드를 가지는 객체에선 외래키를 가지지 않기 때문
- DB엔 중간 테이블 추가

`@JoinTable(name=이름)`
- 중간 테이블 이름을 관리하고 싶다면 추가할 어노테이션
- 지정하지 않으면 자동으로 이름이 정해짐(위 경우 producer_product)
- 중간 테이블엔 관계를 맺은 두 테이블의 id값이 설정되어 있음

---

### 2. 다대다 양방향 매핑

양방향 매핑을 위해 `Product` 엔티티에서 관계 표시

```java
@ManyToMany
@ToString.Exclude
private List<Producer> producers = new ArrayList<>();

public void addProducer(Producer producer){
    this.producers.add(producer);
}
```

- 필요하다면 `mappedBy`를 통해 연관관계의 주인 설정 가능

---
<br>

## Ⅴ. 영속성 전이

### 1. 영속성 전이란?

> 특정 엔티티의 영속성 상태를 변경할 때 그 엔티티와 연관된 엔티티의 영속성도 변경하는 것을 의미

연관관계 어노테이션에서 `casacade` 속성을 통해 지정
- 지정할 수 있는 타입은 다음과 같다.

|All|모든 영속성 전이를 적용|
|PERSIST|부모 엔티티를 저장할 때 자식 엔티티도 저장|
|MERGE|부모 엔티티를 병합할 때 자식 엔티티도 병합|
|REMOVE|부모 엔티티를 삭제할 때 자식 엔티티도 삭제|
|REFRESH|부모 엔티티를 새로 고칠 때 자식 엔티티도 새로 고침|
|REFRESH|부모 엔티티를 준영속 상태로 만들 때 자식 엔티티도 준영속 상태로 만듦|

- 영속성 전이에 사용되는 타입은 엔티티 생명주기와 연관

---

### 2. 영속성 전이 적용

새로운 공급업체와 계약하여 다수의 새 상품을 입고시키는 상황을 구현
- 엔티티를 데이터베이스가 추가하는 경우
- 영속성 전이 타입을 `PERSIST`로 설정
  
`Provider` 엔티티의 `productList` 필드를 다음과 같이 변경한다.

```java
@OneToMany(mappedBy = "provider", cascade = CascadeType.PERSIST)
@ToString.Exclude
private List<Product> productList = new ArrayList<>();
```

이후 `Provider` 객체에 `Product` 객체들을 추가한 후 DB에 저장하면 `Product` 객체도 자동으로 저장된다.

---

### 3. 고아 객체

> 부모 엔티티와 연관관계가 끊어진 엔티티

JPA에는 이러한 고아 객체를 자동으로 제거하는 기능이 존재
- 자식 엔티티가 다른 엔티티와 연관관계를 가지고 있다면 이 기능은 사용하지 않는 것이 좋음

고아 객체 제거 기능을 사용하기 위해서 `Provider` 엔티티의 `productList` 필드를 다음과 같이 변경

```java
@OneToMany(mappedBy = "provider", cascade = CascadeType.PERSIST, orphanRemoval = true)
@ToString.Exclude
private List<Product> productList = new ArrayList<>();
```

`orphanRemoval = true`
- 고아 객체를 제거하는 기능

---
<br>

## ※ 로딩 전략

### 1. 지연 로딩(Lazy Loading)

> 실제로 데이터가 필요할 때까지 데이터를 로드하지 않는 방식

#### 장점

- 성능 최적화: 처음에는 필요한 최소한의 데이터만 로드하므로, 초기 데이터베이스 접근이 빠름
- 메모리 절약: 필요한 데이터만 로드하기 때문에 메모리 사용량이 줄어듦
- 효율적 네트워크 사용: 대규모 데이터셋을 다룰 때 불필요한 데이터 전송을 피할 수 있음

#### 단점
추가 쿼리: 필요한 시점마다 추가적인 데이터베이스 쿼리가 발생할 수 있어, 다수의 쿼리가 실행될 수 있음
지연 발생: 필요한 데이터를 호출할 때마다 지연이 발생할 수 있음

---

### 2. 즉시 로딩(Eager Loading)

> 처음부터 필요한 모든 데이터를 한 번에 로드하는 방식

#### 장점

- 한 번의 쿼리: 관련된 모든 데이터를 한 번의 쿼리로 가져오기 때문에, 추가적인 데이터베이스 접근이 줄어듦
- 예측 가능성: 모든 데이터가 미리 로드되어 있으므로, 데이터를 접근할 때 지연이 발생하지 않음
  
#### 단점
- 초기 로딩 시간 증가: 처음에 모든 데이터를 로드하기 때문에 초기 데이터베이스 접근 시간이 길어질 수 있음
- 메모리 사용량 증가: 불필요한 데이터를 포함하여 모든 데이터를 로드하므로, 메모리 사용량이 증가
- 비효율적인 네트워크 사용: 필요한 데이터만 로드하는 것이 아니라 모든 데이터를 로드하기 때문에, 네트워크 사용이 비효율적