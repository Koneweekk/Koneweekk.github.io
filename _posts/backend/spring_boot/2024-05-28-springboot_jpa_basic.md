---
title: Spring Boot - JPA 기본
date: 2024-05-28 15:00:00 +09:00
categories: [Back-End, Spring Boot]
tags: [Back-End, Java, Spring Boot, JPA]
---

## Ⅰ. JPA란?

### 1. ORM(Object Relational Mapping)

> 객체 관계 매핑 : 객체(클래스)와 RDB의 테이블을 자동으로 매핑하는 방법

- 객체 지향 언어의 클래스와 RDB 테이블의 불일치와 제약사항을 해결하는 역할

![ORM](https://blog.kakaocdn.net/dn/cSTnOP/btrbvwAsfaf/9AR99jMMUqLcEWWLzPS3kk/img.png)

#### 장점

생산성 향상
- SQL 쿼리를 작성하는 대신 ORM을 사용하면 데이터베이스와의 상호 작용을 객체 지향 코드로 처리
- 기본적인 CRUD(Create, Read, Update, Delete) 작업을 자동으로 처리

유지보수성 향상
- 데이터베이스 접근 로직이 객체 지향 코드로 캡슐화되어 있어 코드의 변경과 확장이 용이
- 이는 코드의 가독성을 높이고 유지보수를 쉽게 만듦

데이터베이스 독립성
- ORM을 사용하면 애플리케이션이 특정 데이터베이스에 종속되지 않게 됨
- 데이터베이스를 교체하거나 다른 데이터베이스를 사용할 때 코드 변경이 최소화

#### 단점

제한된 기능
- ORM이 자동으로 생성하는 쿼리는 때때로 최적화가 부족할 수 있음
- ORM은 데이터베이스의 모든 기능을 추상화하지 못할 수 있으며, 특정 데이터베이스의 고유 기능을 사용할 때 제한
- ORM은 추상화 계층을 추가하기 때문에, 복잡한 쿼리나 대량의 데이터 처리 시 성능이 저하

관점의 불일치
- 세분성 : ORM의 자동 설계방법에 따라 테이블과 엔티티 클래스의 수가 불일치
- 상속성 : DB에는 상속이란 개념이 없음
- 식별성 : 자바는 두 객체의 값이 같아도 다르다고 판단 가능
- 연관성 : DB는 객체를 참조하는 방식이 아닌 외래키를 삽입함으로써 연관성 표시
- 관계의 방향성 : 자바는 객체를 참조할 때 방향성이 존재하지만 DB는 존재하지 않음
- 탐색 : 자바(객체와 객체를 그래프 형태로 연결)와 RDBMS(쿼리를 최소화하고 조인을 통해 값 추출)는 값에 접근하는 방식이 다름.

---

### 2. JPA

> 자바 진영의 ORM 기술 표준으로 채택된 인터페이스의 모음

- ORM의 더 구체화된 스펙을 포함
- 실제로 동작하는 것이 아닌 어떻게 동작하는지 메커니즘을 정리한 표준 명세
- 개발자 대신 적절한 SQL을 생성하고 데이터 베이스를 조작
- 내부적으로 JDBC 사용

---

### 3. Hibernate

> 자바 ORM 프레임워크로, JPA 구현체 중 하나

![Hibernate](https://miro.medium.com/v2/resize:fit:404/1*-9P6g_72e8arRyT2WeWPow.png)

#### Sping Data JPA
- JPA를 편리하게 사용할 수 있도록 지원하는 스프링 하위 프로젝트
- CRUD에 필요한 인터페이스 제공
- 하이버네이트에서 자주 사용되는 기능을 더 쉽게 사용할 수 있게 구현한 라이브러리

---

### 4. 영속성 컨텍스트

> 애플리케이션과 DB 사이에서 엔티티와 레코드의 괴리를 해소하고, 객체를 보관하는 기능

![영속성 컨텍스트](https://user-images.githubusercontent.com/69145799/122075055-baa61580-ce34-11eb-9c96-60e8b770d166.png)

- 엔티티 객체가 영속성 컨텍스트에 들어오면 JPA는 엔티티 객체의 매핑 정보를 데이터베이스에 반영
- 엔티티 객체가 영속성 컨텍스트에 들어와 JPA의 관리 대상이되면 영속 객체라 부름

#### 엔티티 매니저

> 엔티티를 관리하는 객체

- 데이터베이스에 접근해서 CRUD 작업을 수행
- 엔티티 매니저 팩토리(`EntityManagerFactory`)가 만듦
- 엔티티를 영속성 컨텍스트에 추가해 영속 객체로 만듦
- 영속성 컨텍스트와 데이터베이스를 비교하여 실제 데이터베이스를 대상으로 작업

#### 엔티티 매니저 팩토리
- 하이버네이트에선 별도의 설정을 거친 후 사용해야하는 객체
- 싱글톤 패턴을 가짐

#### 엔티티 생명주기

- 비영속 : 영속성 컨텍스트에 추가되지 않은 엔티티 객체의 상태
- 영속 : 영속성 컨텍스트에 의해 객체가 관리되는 상태
- 준영속 : 영속성 컨텍스트에 의해 관리되던 엔티티 객체가 컨텍스트와 분리된 상태
- 삭제 : 데이터베이스에서 레코드를 삭제하기 위해 영속성 컨텍스트에 삭제 요청

---
<br>

## Ⅱ. 데이터베이스 연동

### 1. DB 연결 설정

연동시킬 DB에 대한 설정과 하이버네이트 사용 옵션을 application.properties 파일에서 설정

```
# 연결할 데이터베이스에 대한 설정
spring.datasource.url=jdbc:mysql://localhost:3306/springboot
spring.datasource.username=root
spring.datasource.password=1004
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

# 하이버네이트를 사용할 때 활성화할 수 있는 선택사항
spring.jpa.hibernate.ddl-auto=create
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true;
```

`ddl-auto`
- 데이터베이스를 자동으로 조작하는 옵션
- `create` : 애플리케이션 가동 후 `sessionFactory`가 실행될 때 기존 테이블 지우고 새로 생성
- `create-drop` : `create`와 동일, 대신 애플리케이션 종료 시 테이블 삭제
- `update` : `sessionFactory`가 실행될 때 변경된 스키마 갱신
- `validate` : 객체를 검사하지만 스키마는 건드리지 않음. DB와 객체 정보가 다르면 에러 발생
- `none` : ddl-auto 기능을 사용하지 않음
- `create`, `create-drop`, `update`는 개발환경에서 사용하지만 운영환경에선 사용하지 않음

`show-sql`
- 로그에 하이버네이트가 생성한 쿼리문을 출력

`format_sql`
- 사람이 보기 좋은 형태로 쿼리문을 로그에 출력

---

### 2. Entity

> 데이터베이스의 테이블에 대응하는 클래스

- 데이터베이스에 쓰일 테이블과 칼럼을 정의

```java
package com.springboot.jpa.entity;

import java.time.LocalDateTime;

import jakarta.persistence.Column;
import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import jakarta.persistence.Table;

@Entity
@Setter
@Getter
@Builder
@NoArgsConstructor
@AllArgsConstructor
@Table(name="product")
public class Product {
  
  @Id
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  private Long number;

  @Column(nullable = false)
  private String name;

  @Column(nullable = false)
  private String price;

  @Column(nullable = false)
  private String stock;

  private LocalDateTime createdAt;

  private LocalDateTime updatedAt;
}
```
- 위와 같이 클래스 생성하면 설정에 따라 DB에 테이블이 자동으로 만들어짐
- 어노테이션을 사용하여 테이블과 매핑하거나 테이블 간 연관관계 정의 가능

#### `@Entity`
- 해당 클래스가 엔티티임을 명시하기 위한 어노테이션
- 클래스 자체는 테이블과 1:1로 매칭되며, 해당 클래스의 인스턴스는 하나의 레코드

#### `@Table`
- 엔티티 클래스는 테이블과 매핑되므로 특별한 경우가 아니면 불필요
- 클래스의 이름과 테이블의 이름을 다르게 지정해야하는 경우에 사용

#### `@Id`
- 테이블의 기본값이 되는 칼럼에 붙이는 어노테이션
- 모든 엔티티에는 `@Id`가 꼭 필요

#### `@GeneratedValue`
- 해당 필드의 값 자동 생성 방식을 결정
- 일반적으로 `@Id` 어노테이션과 함께 사용
- 사용 X : 교유한 기본값을 생성, 내부 정해진 규칙에 의해 기본값 생성
- `AUTO` : `@GeneratedValue`의 기본값, 기본값을 사용하는 DB에 맞게 자동 생성
- `IDENTITY` : 기본값 생성을 데이터베이스에 위임, DB의 `AUTO_INCREMENT`를 사용
- `SEQUENCE` : `@SequenceGenerator` 어노테이션으로 식별자 생성기 설정 후, 이를 통해 자동 주입
  - `SequenceGenerator`는 `name.sequenceName.allocationSzie`을 활용하여 정의
  - `@GeneratedValue`에 생성기를 설정
- `TABLE` : 어떤 DBMS를 사용하더라도 동일하게 동작하기를 원할 경우 사용
  - 식별자로 사용할 숫자의 보관 테이블을 별도로 생성해 엔티티를 생성할 때마다 값을 갱신
  - `@TableGenerator`를 통해 테이블 정보를 설정

#### `@Column`
- 엔티티 클래스 필드는 자등올 테이블 칼럼과 매핑되므로 별다른 설정이 없으면 생략 가능
- `name` : 칼럼의 이름 지정
- `nullable` : 칼럼이 `NULL`값을 가질수 있는 여부(기본값 : `true`)
- `unique` : 고유값 여부(기본값 : `false`)
- `length` : 문자열 컬럼의 최대 길이 지정 (기본값 : 255)

#### `@Transient`
- 엔티티 클래스에는 존재하지만 DB에는 필요없을 경우 사용

---

### 3. Repository

> 엔티티가 생성한 데이터베이스에 접근하는데 사용

```java
package com.springboot.repository;

import org.springframework.data.jpa.repository.JpaRepository;

import com.springboot.jpa.entity.Product;

public interface ProductRepository extends JpaRepository<Product, Long>{

}
```

- `JpaRepository`을 상속받아 기존 메서드들을 별다른 쿠현없이 손쉽게 활용 가능
- 대상 엔티티와 기본값 타입을 지정해야함

#### 커스텀 메서드
- 몇가지 명명 규칙에따라 커스텀 메서드도 생성 가능
- `select` 쿼리에서 많이 활용

|키워드|설명|
|---|---|
|FindBy|SQL문의 where 절 역할을 수행|
|And|두 조건을 AND 연산으로 결합|
|Or|두 조건을 OR 연산으로 결합|
|Is, Equals|두 값이 같은지 비교|
|Between|두 값 사이에 있는지 비교|
|LessThan|값이 작은지 비교|
|LessThanEqual|값이 작거나 같은지 비교|
|GreaterThan|값이 큰지 비교|
|GreaterThanEqual|값이 크거나 같은지 비교|
|After|날짜가 특정 값 이후인지 비교|
|Before|날짜가 특정 값 이전인지 비교|
|IsNull|값이 null인지 비교|
|IsNotNull|값이 null이 아닌지 비교|
|Like|문자열이 특정 패턴과 일치하는지 비교 (와일드카드 사용)|
|NotLike|문자열이 특정 패턴과 일치하지 않는지 비교|
|StartingWith|문자열이 특정 값으로 시작하는지 비교|
|EndingWith|문자열이 특정 값으로 끝나는지 비교|
|Containing|문자열이 특정 값을 포함하는지 비교|
|OrderBy|결과를 정렬|
|In|값이 컬렉션에 포함되는지 비교|
|NotIn|값이 컬렉션에 포함되지 않는지 비교|

---
<br>

## Ⅲ. DAO

### 1. DAO란?

> 데이터베이스에 접근하기 위한 로직을 관리하기 위한 객체

- 비즈니스 로직의 동작 과정에서 데이터를 조작하는 기능은 DAO 객체가 수행
- 스프링 데이터 JPA에선 리포지토리가 이를 대체
- 규모가 작은 서비스에선 생략하기도 함

#### 사용하는 이유
- 데이터를 다루는 중간 계층을 다루는 것이 비즈니스 로직 유지보수 측면에 용이한 경우가 많음
- 비즈니스 로직을 수행하는 과정에서 데이터베이스에 관한 작업을 처리하는 것은 기능 분리 및 관리에 불리

---

### 2. DAO 설계

- DAO에서 활용할 메서드들 정의
- 메서드의 리턴값으론 데이터 객체를 전달

```java
@Component
@RequiredArgsConstructor
public class ProductDAO {

  private final ProductRepository productRepository;

  public Product insertProduct(Product product) {
    return null;
  }

  public Product selectProduct(Long number) {
    return null;
  }

  public Product updateProduct(Long number, String name) throws Exception {
    return null;
  }

  public void deleteProduct(Long number) throws Exception {
    return null;
  }
  
}
```

- `@Component`를 통해 스프링이 관리하는 빈으로 등록
- `@RequiredArgsConstructor`을 통해 레포지토리를 자동으로 의존성 주입

---

### 3. 메서드 구현

#### Insert

```java
@Override
public Product insertProduct(Product product) {
  Product savedProduct = productRepository.save(product);
  return savedProduct;
}
```

- `save` 메서드 활용

#### Select

```java
@Override
public Product selectProduct(Long number) {
  Product selectedProduct = productRepository.findById(number).orElse(null);
  return selectedProduct;
}
```

- `findById` 메서드 활용. 매개변수로 id값을넘김
- `orElse(null)` 값이 존재하지 않으면 null값 반환
- 영속성 컨텍스트의 캐시에서 값 조회 후, 존재하지 않으면 데이터베이스 조회

#### Update

```java
@Override
public Product updateProduct(Long number, String name) throws Exception {
  Product selectedProduct = productRepository.findById(number).orElse(null);

  Product updatedProduct;
  if (selectedProduct != null) {
    Product product = selectedProduct;
    product.setName(name);
    product.setUpdatedAt(LocalDateTime.now());
    updatedProduct = productRepository.save(product);
  } else {
    throw new Exception();
  }

  return updatedProduct;
}
```

- JPA는 값을 갱신할 때도 `save` 메서드 활용
- 영속성 컨텍스트가 유지되는 상태에서 객체값을 바꾸고 `save`하면 더티 체크 실행
- 더티 체크(Dirty Check) : 변경 감지 로직. 변경이 감지시 해당하는 레코드를 업데이트
- 해당 메서드는 내부적으로 `@Transactional` 어노테이션 선언(메서드 종료시 자동으로 `flush()` 실행)

#### Delete

```java
@Override
public void deleteProduct(Long number) throws Exception {
  Product selectedProduct = productRepository.findById(number).orElse(null);

  if (selectedProduct != null) {
    Product product = selectedProduct;
    productRepository.delete(product);
  } else {
    throw new Exception();
  }
}
```

- 데이터 삭제를 위해선 영속성 컨텍스트에서 객체를 가지고 와야함
- `delete()` 메서드를 통해 삭제 요청

---
<br>

## Ⅲ. 연동된 데이터베이스 활용

### 1. DTO

> 계층 간 데이터 전송을 위한 객체

서비스 클래스를 작성하기 전 필요한 DTO 클래스를 생성

```java
package com.springboot.jpa.dto;

import lombok.Getter;
import lombok.Setter;

@Getter
@Setter
public class ProductDto {
  private String name;
  private int price;
  private int stock;
}
```

```java
package com.springboot.jpa.dto;

import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;

@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class ProductResponseDto {
  private Long number;
  private String name;
  private int price;
  private int stock;
}
```

---

### 2. 서비스 클래스

> 도메인 모델을 활용해 애플리케이션에서 제공하는 핵심 기능을 제공

```java
@Component
@RequiredArgsConstructor
public class ProductService {

  private final ProductDAO productDAO;

  // Product READ
  public ProductResponseDto getProduct(Long number) {

    Product product = productDAO.selectProduct(number);
    ProductResponseDto productResponseDto = ProductResponseDto.builder()
                                                              .number(product.getNumber())
                                                              .name(product.getName())
                                                              .price(product.getPrice())
                                                              .stock(product.getStock())
                                                              .build();
                                                                
    return productResponseDto;
  }

  // Product CREATE
  public ProductResponseDto createProduct(ProductDto productDto) {
    Product product = Product.builder()
                            .name(productDto.getName())
                            .price(productDto.getPrice())
                            .stock(productDto.getStock())
                            .build();
    
    Product savedProduct = productDAO.insertProduct(product);

    ProductResponseDto productResponseDto = ProductResponseDto.builder()
                                                            .number(savedProduct.getNumber())
                                                            .name(savedProduct.getName())
                                                            .price(savedProduct.getPrice())
                                                            .stock(savedProduct.getStock())
                                                            .build();

    return productResponseDto;
  }

  // Product Update
  public ProductResponseDto updateProduct(Long number, String name) throws Exception {

    Product updatedProduct = productDAO.updateProduct(number, name);

    ProductResponseDto productResponseDto = ProductResponseDto.builder()
                                                            .number(updatedProduct.getNumber())
                                                            .name(updatedProduct.getName())
                                                            .price(updatedProduct.getPrice())
                                                            .stock(updatedProduct.getStock())
                                                            .build();

    return productResponseDto;
  }

  // Product Delete
  public void deleteProduct(Long number) throws Exception {
    productDAO.deleteProduct(number);
  }

}
```

- DAO의 메서드를 활용하여 DB 조작 후 결과를 반환받음
- Entity 객체와 DTO 객체 사이의 변환 작업을 수행
- 최종적으로 DTO 객체를 반환

---

### 3. 컨트롤러 클래스

> 비즈니스 로직과 클라이언트의 요청을 연결

```java
@RestController
@RequestMapping("/product")
@RequiredArgsConstructor
public class ProductController {
  
  private final ProductService productService;

  @GetMapping("/{variable}")
  public ResponseEntity<ProductResponseDto> getProduct(@PathVariable("variable") String variable) {
    Long number = Long.parseLong(variable);
    ProductResponseDto productResponseDto = productService.getProduct(number);

    return ResponseEntity.status(HttpStatus.OK).body(productResponseDto);
  }

  @PostMapping()
  public ResponseEntity<ProductResponseDto> postProduct(@RequestBody ProductDto productDto) {
    ProductResponseDto productResponseDto = productService.createProduct(productDto);

    return ResponseEntity.status(HttpStatus.OK).body(productResponseDto);
  }

  @PutMapping("/{variable}")
  public ResponseEntity<ProductResponseDto> putProduct(@PathVariable("variable") String variable, @RequestBody String name) throws Exception {
    Long number = Long.parseLong(variable);
    ProductResponseDto productResponseDto = productService.updateProduct(number, name);
      
    return ResponseEntity.status(HttpStatus.OK).body(productResponseDto);
  }

  @DeleteMapping("/{variable}")
  public ResponseEntity<String> deleteProduct(@PathVariable("variable") String variable) throws Exception {
    Long number = Long.parseLong(variable);
    productService.deleteProduct(number);
      
    return ResponseEntity.status(HttpStatus.OK).body("정상적으로 삭제되었습니다.");
  }
}
```

- 서비스 클래스의 메서드를 적절히 활용
- `ResponseEntity`를 활용하여 상태 코드와 객체를 response에 전달