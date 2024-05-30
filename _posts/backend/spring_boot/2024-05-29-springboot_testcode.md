---
title: Spring Boot - 테스트 코드
date: 2024-05-29 12:00:00 +09:00
categories: [Back-End, Spring Boot]
tags: [Back-End, Java, Spring Boot, Test Code]
---

## Ⅰ. 테스트 코드 개요

### 1. 테스트 코드란?

> 말그대로 우리가 작성한 코드 혹은 로직 자체를 테스트하기 위한 코드

- 애플리케이션의 다양한 부분을 다양한 방식으로 테스트
- 버그를 조기에 발견하고 코드의 품질을 유지하는 데 중요한 역할

---

### 2. 테스트 코드를 작성하는 이유

#### 개발 과정에서 문제를 미리 발견
- 코드에 잠재된 문제를 발견하는데 큰 도움
- 문제가 발생할 수 있는 여러 상황에 맞춰 테스트 코드를 작성해야함

#### 리팩토링 리스크 감소
- 일반적으로 애플리케이션 개발 후 서비스 업데이트를 위해 계속해서 코드 추가 및 수정
- 이러한 과정은 연관된 코드에 영향을 주는 작업
- 테스트 코드가 작성되어 있다면 테스트 코드 실행만으로 부작용에 대비할 수 있음

#### 명세 문서로의 기능 수행
- 협업시 타인의 코드 이해와 충돌 방지를 위한 명세로서 유용한 기능
- 동작 검증을 위한 테스트 코드를 애플리케이션 코드와 비교해보며 작성자의 의도 파악 용이
- 애플리케이션을 직접 테스트하는 것보다 빠르게 진행

---

### 2. 단위 테스트와 통합 테스트

#### 단위 테스트

> 애플리케이션의 개별 모듈을 독립적으로 확인

- 가장 작은 단위의 테스트 방식
- 일반적으로 메서드 단위로 테스트 수행
- 메서드 호출이 의도한 결과값을 반환하는지 확인
- 비용이 적게 들기 때문에 테스트 피드백을 빠르게 받음

#### 통합 테스트

> 애플리케이션을 구성하는 다양한 모듈을 결합해 전체 로직이 의도한 대로 작동하는지 확인

- 모듈을 통합하는 과정에서 호환성을 포함해 애플리케이션의 정상적인 동작을 확인
- 여러 모듈을 함께 테스트해 정상적인 로직 수행이 가능한지 확인
- 단위 테스트에 비해 데이터베이스나 네트워크 같은 외부 요인들도 포함
- 테스트 비용이 크다는 단점이 존재

#### ※ 테스트 비용

- 금전적인 비용, 시간, 인력 등 개발에 필요한 모든 것을 포괄
- 통계적으로 개발 과정에 60%, 테스트 과정에 40%의 비용이 서비스 개발 시 발생하는 것을 알려짐

---

### 3. Given-When-Then 패턴

- 테스트 주도 개발에서 파생된 BDD(Behavior-Driven-Development)를 통해 탄생한 테스트 접근 방식
- 일반적으로 인수 테스트(비교적 많은 환경을 포함해 테스트)에 적합
- 불필요하게 코드가 길어지기 때문에 단위 테스트에선 잘 사용되지 않음

#### Given
- 테스트를 수행하기 전에 테스트에 필요한 환경을 설정하는 단계
- 테스트에 필요한 변수를 정의하거나 Mock 객체를 통해 특정 상황에 대한 행동을 정의

#### When
- 테스트 목정르 보여주는 단계
- 실제 테스트 코드가 포함되며, 테스트를 통한 결괏값을 가져오게 됨

#### Then
- 테스트의 결과를 검증하는 단계
- 일반적으로 When 단계에서 나온 결괏값 검증을 수행

---

### 4. F.I.R.S.T

> 좋은 테스트 코드의 5가지 규칙

#### Fast
- 테스트는 빠르게 수행되어야함

#### Isolated
- 하나의 테스트 코드는 목적 대상에 대해서만 수행되어야함

#### Repeatable
- 어떤 환경에서도 반복 가능해야함

#### Self-Validating
- 그 자체만으로도 테스트의 검증이 완료되어야함
- 테스트의 성공 실패 여부확인할 수 있는 코드가 동반되어야함

#### Timely
- 테스트하려는 애플리케이션 코드 구현 전에 완성되어야함

---
<br>

## Ⅱ. JUnit을 활용한 테스트 코드 작성

### 1. JUnit이란?

> 자바 언어에서 사용되는 대표적인 테스트 프레임워크

- 단위테스트 뿐만 아니라 통합 테스트 기능도 제공
- 어노테이션 기반 테스트 방식을 지원함으로써 간편한 사용 가능
- 단정문(assert)를 통해 테스트 케이스의 기대값이 정상적으로 도출됐는지 확인 가능

---

### 2. JUnit이란?

> 크게 Jupiter, Platform, Vintage 로 구성

![JUnit 세부 모듈](https://static.packt-cdn.com/products/9781787285736/graphics/93231e34-6b81-4e81-9d94-e83e7742885e.png)

#### JUnit Platform
- JVM에서 테스트를 시작하기 위한 뼈대 역할
- 테스트 엔진 : 테스트를 발견, 테스트 수행, 그 결과를 보고
- 각종 IDE와의 연동을 보조
- TestEngine API, Console Launcehr, JUnit 4 Based Runner등이 포함

#### JUnit Jupiter
- JUnit 5에 대한 테스트 엔진 API의 구현체를 포함
- Jupiter Engine : Jupiter API를 활용해서 작성한 테스트 코드를 발견하고 실행하는 역할 수행
  
#### JUnit Vintage
- JUnit 3, 4에 대한 테스트 엔진 API를 구현

---

### 3. JUnit 설정

build.gradle에 의존성이 잘 주입되었는지 확인

```
dependencies {
  ...
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
  ...
}
```
`spring-boot-starter-test`가 제공하는 대표적인 라이브러리는 다음과 같다.

- JUnit 5 (Jupiter): 단위 테스트를 작성하고 실행하는 데 사용되는 주요 테스트 프레임워크
- Spring Test & Spring Boot Test: 스프링과 스프링 부트를 테스트하기 위한 도구들을 제공
- AssertJ: 다양한 단정문(assertion) 기능을 제공하는 라이브러리
- Hamcrest: 매처 기반의 단언을 작성할 수 있는 라이브러리
- Mockito: 모킹 프레임워크로, 의존성을 가짜(mock)로 만들어 단위 테스트를 수행
- JSONassert: JSON 데이터를 검증할 수 있는 라이브러리
- JsonPath: JSON 데이터를 탐색하고 추출할 수 있는 라이브러리

---

### 4. JUnit 생명 주기

생명주기와 관련되어 테스트 순서에 관여하게 되는 대표적인 어노테이션들은 다음과 같다.

- `@BeforeAll` : 모든 테스트 메서드가 실행되기 전에 한 번 실행(반드시 static 메서드)
- `@AfterAll` : 모든 테스트 메서드가 실행된 후에 한 번 실행(반드시 static 메서드)
- `@BeforeEach` : 각 테스트 메서드가 실행되기 전에 실행
- `@AfterEach` : 각 테스트 메서드가 실행된 후에 실행
- `@Test` : 테스트 메서드를 표시. 이 메서드는 테스트 프레임워크에 의해 실행

```java
package com.springboot.test;

import org.junit.jupiter.api.AfterAll;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeAll;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Disabled;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

public class TestLifeCycle {

  @BeforeAll
  static void beforAll() {
    System.out.println("## BeforeAll Anotation 호출 ##");
    System.out.println();
  }
  
  @AfterAll
  static void afterAll() {
    System.out.println("## AfterAll Anotation 호출 ##");
    System.out.println();
  }

  @BeforeEach
  void beforeEach() {
    System.out.println("## BeforeEach Anotation 호출 ##");
    System.out.println();
  }

  @AfterEach
  void afterEach() {
    System.out.println("## AfterEach Anotation 호출 ##");
    System.out.println();
  }

  @Test

  void test1() {
    System.out.println("test1 시작");
    System.out.println();
  }

  @Test
  @DisplayName("test2입니다")
  void test2() {
    System.out.println("test2 시작");
    System.out.println();
  }

  @Test
  @Disabled
  void test3() {
    System.out.println("test3 시작");
    System.out.println();
  }
}
```

위 코드를 test 폴더 안에 작성하고 실행하면 결과는 다음과 같다.

```
## BeforeEach Anotation 호출 ##

test1 시작

## AfterEach Anotation 호출 ##

## BeforeEach Anotation 호출 ##

test2 시작

## AfterEach Anotation 호출 ##

## AfterAll Anotation 호출 ##
```

- `@Disabled`를 선언한 `test3` 메서드는 실행되지 않은 것을 볼 수 있다.

---

## Ⅲ. JUnit을 통한 컨트롤러 테스트

### 1. 테스트 클래스 생성

현재 컨트롤러의 코드는 다음과 같다

```java
@RestController
@RequestMapping("/product")
@RequiredArgsConstructor
public class ProductController {
  
  private final ProductService productService;

  @GetMapping()
  public ResponseEntity<ProductResponseDto> getProduct(@RequestParam("number") Long number) {
    ProductResponseDto productResponseDto = productService.getProduct(number);

    return ResponseEntity.status(HttpStatus.OK).body(productResponseDto);
  }

  @PostMapping()
  public ResponseEntity<ProductResponseDto> postProduct(@RequestBody ProductDto productDto) {
    ProductResponseDto productResponseDto = productService.createProduct(productDto);

    return ResponseEntity.status(HttpStatus.OK).body(productResponseDto);
  }
}
```

이를 테스트하기 위한 컨트롤러 클래스의 초기 코드는 다음과 같다.
- `test/java/com/springboot/test` 패키지에 `controller` 패키지를 생성 후 파일 생성
- 테스트하는 입장에서 `productService`는 외부 요인에 해당
- 독립적인 테스트 코드를 위해선 `Mock` 객체 활용 필요

```java
@WebMvcTest(ProductController.class)
public class ProductControllerTest {

  @Autowired
  private MockMvc mockMvc;

  @MockBean
  ProductService productService;

  @Test
  void testGetProduct() throws Exception {
    ...
  }

  @Test
  void testPostProduct() {
    ...
  }
}
```

#### `@WebMvcTest(테스트 대상 클래스.class)`
- 웹에서 사용되는 요청과 응답에 대한 테스트를 수행 가능
- 대상 클래스만 로드해 테스트를 수행
- 이 어노테이션을 사용한 테스트는 보통 슬라이스 테스트라 부름(단위 테스트와 통합 테스트의 중간 개념)

#### `MockMvc`
- Spring MVC 컨트롤러를 실제 서버 없이 테스트할 수 있도록 도와줌
- HTTP 요청과 응답을 모의하여 컨트롤러의 동작을 검증

#### `@MockBean`
- 실제 빈 객체가 아닌 가짜 객체를 생성해 주입하는 역할
- 이 어노테이션이 선언된 객체는 실제 행위를 수행하지 않음
- 해당 객체는 개발자가 `Mockito`의 `given` 메서드를 통해 동작을 정의해야함

#### `@Test`
- 테스트 코드가 포함돼 있다고 선언하는 어노테이션

---

### 2. GET 메서드 테스트

```java
@Test
@DisplayName("MockMvc를 통한 Product 데이터 가져오기 테스트")
void testGetProduct() throws Exception {

    // 서비스의 getProduct 메서드를 모킹하여, 123L 파라미터로 호출될 때 미리 정의된 ProductResponseDto를 반환하도록 설정
    BDDMockito.given(productService.getProduct(123L)).willReturn(
      new ProductResponseDto(123L, "pen", 5000, 2000)
    );

    // 테스트에 사용할 제품 ID를 문자열로 정의
    String productId = "123";

    // MockMvc를 사용하여 /product 엔드포인트에 number=123 쿼리 파라미터와 함께 GET 요청을 보냄
    mockMvc.perform(MockMvcRequestBuilders.get("/product?number=" + productId))
          // HTTP 응답 상태 코드가 200(OK)인지 확인
          .andExpect(MockMvcResultMatchers.status().isOk())
          // 응답 JSON에 "number" 필드가 존재하는지 확인
          .andExpect(MockMvcResultMatchers.jsonPath("$.number").exists())
          // 응답 JSON에 "price" 필드가 존재하는지 확인
          .andExpect(MockMvcResultMatchers.jsonPath("$.price").exists())
          // 응답 JSON에 "stock" 필드가 존재하는지 확인
          .andExpect(MockMvcResultMatchers.jsonPath("$.stock").exists())
          // 요청과 응답 내용을 콘솔에 출력
          .andDo(MockMvcResultHandlers.print());

    // productService의 getProduct 메서드가 123L 파라미터로 호출되었는지 검증
    verify(productService).getProduct(123L);
}
```

`BDDMockito.given`
- 이 객체에서 어떤 메서드가 호출되고 어떤 파라미터를 주입받는지 가정

`willReturn`
- `given`에서 넘어온 메서드와 파라미터가 어떤 결과를 리턴할 것인지 정의

`perform`
- 서버로의 URL 요청처럼 통신 테스트 코드를 작성해 컨트롤러를 테스트
- `MockMvcRequestBuilders`에서 제공하는 HTTP 메서드로 URL을 정의해야함
- `ResultActions` 객체가 리턴됨

`MockMvcRequestBuilders`
- GET, POST, PUT, DELETE에 매핑되는 메서드 제공
- `MockHttpServletRequestBuilder` 객체 리턴하며, 요청 정보를 설정 가능

`andExpect`
- `perform`의 결과값 검증을 수행
- `MockMvcResultMatchers` 클래스의 메서드를 통해 생성하는 `ResultMatcher`를 활용
  
`andDo`
- 요청과 응답의 전체 내용을 확인

`verify`
- 지정된 메서드가 실행됐는지 검증
- 일반적으로 `given`에 정의된 동작과 대응

---

### 3. POST 메서드 테스트

```java
@Test
@DisplayName("Product 데이터 생성 테스트")
void testPostProduct() throws Exception {

  // 테스트용 ProductDto 객체를 생성. 이 객체는 API 호출 시 본문에 포함될 데이터
  ProductDto productDto = new ProductDto("pen", 5000, 2000);

  // productService.createProduct(productDto) 호출 시 반환될 값을 미리 정의
  // 여기서는 ID가 123L이고, 이름이 "pen", 가격이 5000, 재고가 2000인 ProductResponseDto 객체를 반환하도록 
  BDDMockito.given(productService.createProduct(productDto)).willReturn(
    new ProductResponseDto(123L, "pen", 5000, 2000));

  // Gson 객체를 생성하여 productDto 객체를 JSON 문자열로 변환
  Gson gson = new Gson();
  String content = gson.toJson(productDto);

  // MockMvc를 사용하여 POST 요청을 "/product" 엔드포인트로 보냄
  // 요청 본문은 content로 설정하고, Content-Type은 application/json으로 지정
  mockMvc.perform(MockMvcRequestBuilders.post("/product").content(content).contentType(MediaType.APPLICATION_JSON))
      // HTTP 상태 코드가 200 OK인지 검증
      .andExpect(MockMvcResultMatchers.status().isOk())
      // 응답 JSON에서 number 필드가 존재하는지 검증
      .andExpect(MockMvcResultMatchers.jsonPath("$.number").exists())
      // 응답 JSON에서 name 필드가 존재하는지 검증
      .andExpect(MockMvcResultMatchers.jsonPath("$.name").exists())
      // 응답 JSON에서 price 필드가 존재하는지 검증
      .andExpect(MockMvcResultMatchers.jsonPath("$.price").exists())
      // 응답 JSON에서 stock 필드가 존재하는지 검증
      .andExpect(MockMvcResultMatchers.jsonPath("$.stock").exists())
      // 요청과 응답 내용을 출력
      .andDo(MockMvcResultHandlers.print());

  // productService.createProduct(productDto) 메서드가 한 번 호출되었는지 검증
  verify(productService).createProduct(productDto);
}
```

`Gson`
- 구글에서 개발한 JSON 파싱 라이브러리
- build.gradle에 `implementation 'com.google.code.gson:gson:2.8.6'` 추가
- `toJson` 메서드를 통해 dto를 json 형태로 변형

그 외엔 GET 테스트와 거의 동일

---
<br>

## Ⅳ. JUnit을 통한 서비스 테스트

### 1. 테스트 클래스 생성

```java
public class ProductServiceTest {
  
  private ProductRepository productRepository = Mockito.mock(ProductRepository.class);
  private ProductService productService;

  @BeforeEach
  public void setUpTest() {
    productService = new ProductService(productRepository);
  }

  @Test
  void testGetProduct() {
    
  }

  @Test
  void testCreateProduct() {

  }

}
```

`Mockito.mock(ProductRepository.class)`
- Mockito를 사용하여 ProductRepository 인터페이스의 Mock 객체를 생성
- 실제 객체의 동작을 모방하여 테스트에서 사용
- 테스트의 독립성 유지

`private ProductService productService`
- 테스트 대상 객체 선언

`setUpTest`
- 각 테스트 메서드가 실행되기 전에 `setUpTest` 메서드가 실행
- `ProductService` 객체를 생성하고, 생성자 매개변수로 `productRepository` Mock 객체를 전달


```java
@ExtendWith(SpringExtension.class)
@Import({ ProductService.class })
public class ProductServiceTest2 {

  @MockBean
  ProductRepository productRepository;

  @Autowired
  ProductService productService;
  
}
```

위와 같이 `productRepository` 초기화 작업을 진행할 수도 있다.
- 테스트 어노테이션을 통해 Mock 객체를 생성하고 의존성 주입

`@MockBean`
- 스프링에 Mock 객체를 등록해서 주입
- `Mockito.mock()` : 스프링 빈에 등록하지 않고 직접 객체 초기화
- `Mockito.mock()`이 스프링을 사용하지 않아 속도가 더 빠름

`@ExtendWith(SpringExtension.class)`
- JUnit5 테스트에서 스프링 테스트 컨텍스트를 사용하도록 설정

`@Import({ ProductService.class })`
- `@Autowired`으로 주입받는 `ProductService productService`를 주입받기 위해 사용

---

### 2. GET 로직 테스트

기본적으로 Given-When-Then 패턴을 기반으로 작성

```java
@Test
void testGetProduct() {
    // 테스트용 Product 객체를 생성. 이 객체는 테스트 동안 사용될 기대값
    Product givenProduct = Product.builder()
                                  .number(123L)
                                  .name("pen")
                                  .price(5000)
                                  .stock(2000)
                                  .build();

    // productRepository의 findById 메서드가 호출되었을 때, givenProduct를 반환하도록 설정
    Mockito.when(productRepository.findById(123L))
           .thenReturn(Optional.of(givenProduct));

    // productService의 getProduct 메서드를 호출하여 반환된 ProductResponseDto 객체를 저장
    ProductResponseDto productResponseDto = productService.getProduct(123L);

    // 반환된 ProductResponseDto 객체의 각 필드가 기대 값과 일치하는지 확인
    Assertions.assertEquals(productResponseDto.getNumber(), givenProduct.getNumber());
    Assertions.assertEquals(productResponseDto.getName(), givenProduct.getName());
    Assertions.assertEquals(productResponseDto.getPrice(), givenProduct.getPrice());
    Assertions.assertEquals(productResponseDto.getStock(), givenProduct.getStock());

    // productRepository의 findById 메서드가 123L을 인수로 호출되었는지 검증
    verify(productRepository).findById(123L);
}
```

`when`
- `productRepository` Mock 객체의 `findById` 메서드가 실행될 시 결과값을 정의

`Assertions`
- 테스트 코드에서 예상되는 결과와 실제 결과를 비교하는 데 사
- `assertEquals` : 두 값이 같은지 검사

---

### 3. Post 로직 테스트

```java
@Test
void testCreateProduct() {
  Mockito.when(productRepository.save(any(Product.class)))
  .then(AdditionalAnswers.returnsFirstArg());

  ProductResponseDto productResponseDto = productService.createProduct(new ProductDto("펜", 5000, 2000));

  Assertions.assertEquals(productResponseDto.getName(), "펜");
  Assertions.assertEquals(productResponseDto.getPrice(), 5000);
  Assertions.assertEquals(productResponseDto.getStock(), 2000);

  verify(productRepository).save(any());
}
```

`any()`
- `Mockito`의 `ArgumentMathcers`에서 제공하는 메서드
- 메서드의 실행만을 확인하거나 좀 더 큰 범위의 클래스 객체를 매개변수로 전달할 때 사용

---
<br>

## Ⅴ. JUnit을 통한 레포지토리 테스트

### 1. H2를 사용한 테스트

데이터베이스는 외부 요인에 속함
- 만약 단위 테스트를 고려한다면 DB를 제외할 수 있음
  
데이터베이스를 사용한 테스트는 테스트 과정에서 데이터가 적재됨
- 데이터베이스 사용시 데이터 삭제 코드까지 고려하는 것이 좋음
- 적재된 데이터로 인한 사이드 이펙트까지 고려하는 것이 좋음

기본적으로 별다른 설정이 없다면 테스트 환경에서는 임베디드 데이터베이스를 사용함

```java
@DataJpaTest
public class ProductRepositoryTestByH2 {

  @Autowired
  private ProductRepository productRepository;

  ...
}
```

`@DataJpaTest`
- JPA와 관련된 설정만 로드해서 테스트 진행
- 기본적으로 `@Transactional` 어노테이션 포함
- 테스트 코드가 종료되면 자동으로 DB의 롤백 진행
- 기본값으로 임베디드 DB 사용

---

### 2. 기본 DB를 사용한 테스트

```java
@DataJpaTest
@AutoConfigureTestDatabase(replace = Replace.NONE)
public class ProductRepositoryTestByH2 {

  @Autowired
  private ProductRepository productRepository;

  ...
}
```

`@AutoConfigureTestDatabase`
- `replace` : 테스트 시 사용할 DB를 교체하기 위해 사용
- `Replace.ANY` : 임베디드 메모리 DB 사용(기본값)
- `Replace.NONE` : 실제 사용하는 DB 사용

---

### 3. select, insert 테스트 코드

```java
@Test
void saveTest() {
  //given
  Product product = Product.builder()
                          .name("펜")
                          .price(1000)
                          .stock(2000)
                          .build();
  
  // when
  Product saveProduct = productRepository.save(product);

  // then
  assertEquals(saveProduct.getName(), product.getName());
  assertEquals(saveProduct.getPrice(), product.getPrice());
  assertEquals(saveProduct.getStock(), product.getStock());
}
```

기본적인 insert 테스트

```java
@Test
void selectTest() {
  //given
  Product product = Product.builder()
                          .name("펜")
                          .price(1000)
                          .stock(2000)
                          .build();
  
  Product saveProduct = productRepository.saveAndFlush(product);

  // when
  Product selectProduct = productRepository.findById(saveProduct.getNumber()).get();

  // then
  assertEquals(selectProduct.getName(), product.getName());
  assertEquals(selectProduct.getPrice(), product.getPrice());
  assertEquals(selectProduct.getStock(), product.getStock());
}
```

저장한 데이터를 불러와 비교 수행

---
<br>

## Ⅵ. JaCoCO(Java Code Coverage)

### 1. JaCoCO(Java Code Coverage)란?

> 코드 커버리지(Code Coverage) : 소프트웨어의 테스트 수준이 충분한지 표현하는 지표 중 하나

JaCoCO란 다양한 커버리지 도구 중 가장 보편적을 사용되는 도구이다.
- JUnit 테스트를 통해 애플리케이션의 코드가 얼마나 테스트됐는지 리포트
- 리포트할때 Line과 Branch를 기준으로 작성
- 런타임으로 테스트 케이스를 실행하고 커버리지를 체크하는 방식으로 동작

---

### 2. 플러그인 설정

다음 내용들을 build.gradle 파일에 추가

```gradle
plugins {
	id 'jacoco'
}
```

- JaCoCo 플러그인을 적용

```gradle
tasks.named('test') {
    useJUnitPlatform()
    finalizedBy 'jacocoTestReport' // test가 끝나면 jacocoTestReport 동작
}
```
- test 작업을 JUnit 플랫폼을 사용하도록 설정
- test 작업이 끝나면 jacocoTestReport 작업이 자동으로 실행


```gradle
jacoco {
    toolVersion = "0.8.8"
}
```
- JaCoCo의 버전을 설정

```gradle
jacocoTestReport {
    dependsOn test

    reports {
        // html로 report 생성하기
        // 빌드경로/jacoco/report.html 폴더 내부로 경로 설정
        html.destination file("$buildDir/jacoco/report.html")
    }

    // jacocoTestReport가 끝나면 jacocoTestCoverageVerification 동작 
    finalizedBy 'jacocoTestCoverageVerification'
}
```
- jacocoTestReport 작업은 test 작업에 의존
- HTML 형식의 리포트를 생성
- 리포트의 경로는 $buildDir/jacoco/report.html로 설정
- jacocoTestReport 작업이 끝나면 jacocoTestCoverageVerification 작업이 자동으로 실행
  
```gradle
jacocoTestCoverageVerification {
    violationRules {
        rule {
            enabled = true // 커버리지 적용 여부
            element = 'CLASS' // 커버리지 적용 단위
            // 라인 커버리지 설정
            limit {
                counter = 'LINE'
                value = 'COVEREDRATIO'
                minimum = 0.90
            }

            // 브랜치 커버리지 설정
            limit {
                counter = 'BRANCH'
                value = 'COVEREDRATIO'
                minimum = 0.90
            }

            // 라인 최대 갯수 설정
            limit {
                counter = 'LINE'
                value = 'TOTALCOUNT'
                maximum = 500
            }
        }
    }
}
```

- jacocoTestCoverageVerification 작업에서 커버리지 검증 규칙을 정의
- rule 블록 내에서 여러 가지 커버리지 검증 기준을 설정
- 라인 커버리지 (LINE COVEREDRATIO): 각 클래스의 라인 커버리지가 90% 이상이어야 함
- 브랜치 커버리지 (BRANCH COVEREDRATIO): 각 클래스의 브랜치 커버리지가 90% 이상이어야 함
- 라인 최대 갯수 (LINE TOTALCOUNT): 한 클래스의 라인이 최대 500줄을 넘지 않도록 설정

위처럼 설정할 경우 build/jacoco/report.html 폴더에 테스트 커버리지 리포트들이 생성

---

### 3. 테스트 커버리지 확인

아래 정보는 build/jacoco/report.html/index.html에서 확인 가능

![테스트 커버리지 확인1](/assets/img/post/backend/spring-boot/2024-05-29-springboot_testcode/01.png)

리포트 첫 페이지는 Element가 패키지 단위로 표현되어있음

각 칼럼은 다음과 같은 의미를 지님
- Element : 우측 테스트 커버리지를 측정한 단위 표현. 링크를 통해 세부 사항 파악 가능
- Missed Instruction - Cov : 테스트 수행 후 바이트 코드의 커버리지를 퍼센티지 바로 제공
- Missed Branches - Cov : 분기에 대한 테스트 커버리지를 퍼센티지 바로 제공
- Missed-Cxty : 복잡도에 대한 커버리지 대상 개수와 커버되지 않은 수
- Missed-Lines : 테스트 대상 라인 수와 커버되지 않은 라인 수를 제공
- Missed-Methods : 테스트 대상 메서드 수와 커버되지 않은 메서드 수를 제공
- Missed-Classes : 테스트 대상 클래스 수와 커버되지 않은 메서드 수를 제공
 
![테스트 커버리지 확인2](/assets/img/post/backend/spring-boot/2024-05-29-springboot_testcode/02.png)

위처럼 코드레벨에서의 테스트 커버리지도 확인 가능
- 노란색 : if문과 같은 조건문에서 조건이 부분적으로만 검증됨
- 초록색 : 테스트에서 실행됨
- 빨간색 : 테스트에서 실행되지 않음

---
<br>

## Ⅶ. 테스트 주도 개발(TDD:Test-Driven Development)

### 1. TDD란?

- 반복 테스트를 위용한 소프트웨어 개발 방법론
- 테스트 코드를 먼저 작성한 후 테스트를 통과하는 코드를 작성하는 과정을 반복
- Test-First 개념에 기반을 둔 개발 주기가 짧은 개발 프로세스
- 단순 설계를 중시

---

### 2. TDD의 주기

![tdd](https://i0.wp.com/hanamon.kr/wp-content/uploads/2021/04/TDD-%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%E1%84%8C%E1%85%AE%E1%84%80%E1%85%B5.png?fit=1024%2C680&ssl=1)

- 실패 테스트 작성 : 실패하는 경우의 테스트 코드 먼저 작성
- 테스트를 통과하는 코드 작성 : 테스트 코드를 성공시키기 위한 실제 코드를 작성
- 리팩토링 : 중복 코드를 제거하거나 일반화하는 리팩토링 수행

---

### 3. TDD의 효과

#### 디버깅 시간 단축
- 테스트 코드 기반으로 개발이 진행
- 문제 발생시 어디서 잘못됐는지 확인하기 쉬움

#### 생산성 향상
- 테스트 코드를 통해 지속적인 애플리케이션 코드의 불안정성에 대한 피드백 수용
- 리팩토링 횟수가 줄고 생산성이 높아짐

#### 재설계 시간 단축
- 작성돼 있는 테스트 코드를 기반으로 코드를 작성
- 재설계가 필요한 경우 테스트 코드를 조정함으로써 재설계 시간 단축

#### 기능 추가와 같은 추가 구현이 용이
- 테스트 코드를 통해 의도한 기능을 미리 설계하고 코드 작성
- 목적에 맞는 코드를 작성하는데 비교적 용이