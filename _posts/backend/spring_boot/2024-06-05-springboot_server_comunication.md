---
title: Spring Boot - 서버간 통신
date: 2024-06-05 12:10:00 +09:00
categories: [Back-End, Spring Boot]
tags: [Back-End, Java, Spring Boot]
---

## Ⅰ. RestTemplate

> 스프링에서 HTTP 통신 기능을 손쉽게 사용하도록 설계된 템플릿

- 기본적으로 동기 방식으로 처리
- 비동기 방식으로 사용하고 싶을 경우 `AsyncRestTemplate`을 사용
- 현업에서 많으 쓰이지만 현재 지원 중단된 상태

### 1. RestTemplate의 특징

- HTTP 프로토콜의 메서드에 맞는 여러 메서드 제공
- RESTful 형식을 갖춘 템플릿
- HTTP요청 후 JSON, XML, 문자열 등의 다양한 형식으로 응답받음
- 블로깅 기반의 동기 방식 사용
- 다른 API를 호출할 때 HTTP 헤더에 다양한 값 설정 가능

---

### 2. RestTemplate의 동작 방식

![ RestTemplate의 동작 방식](https://velog.velcdn.com/images/dnrwhddk1/post/8c45e20b-b7ce-4de6-ba79-d09a098d97c9/image.png)

어플리케이션
- 우리가 직접 작성하는 애플리케이션 코드 구현부
- RestTemplate을 선언하고 URI와 HTTP 메서드, Body 등을 설정

`HttpMesaageConverter`
- 외부 API 요청시 `RequestEntity`를 요청 메시지로 변환
  
`ClientHttpRequestFactory`
- 변환된 요청메시지를 `ClientHttpRequest`로 가져온 후 외부 API로 요청

`ResponseErrorHandler`
- 요청에 대한 응답을 받으면 그 오류를 확인
- 오류가 발생하면 `ClientHttpResponse`에서 응답데이터 처리

`HttpMesaageConverter`
- 받은 응답 데이터가 정상적이라면 자바 객체로 변환해서 애플리케이션을 반환

---

### 3. 대표적인 메서드

`getForObject()`
- GET 메서드를 사용
- 응답을 객체로 변환하여 반환

`getForEntity()`
- GET 메서드를 사용
- 응답을 ResponseEntity 객체로 반환
- 응답 본문뿐만 아니라 상태 코드, 헤더 등의 정보도 포함

`postForObject()`
- POST 메서드를 사용
- 응답을 객체로 변환하여 반환

`postForEntity()`
- POST 메서드를 사용
- 응답을 ResponseEntity 객체로 반환

`delete()`
- DELETE 메서드를 사용
- 응답을 받지 않음

`put()`
- PUT 메서드를 사용
- 응답을 받지 않음
 
`patchForObject()`
- PATCH 메서드를 사용
- 응답을 객체로 변환하여 반환

`optionsForAllow`
- 해당 URI에서 지원하는 HTTP 메서드를 조회

`exchange()`
- 임의의 HTTP 메서드를 사용
- HTTP 헤더를 임의로 추가 가능

`excute()`
- 임의의 HTTP 메서드를 사용
- 요청과 응답에 대한 콜백을 수정

---

### 4. GET 형식 RestTemplate 작성

- 일반적으로 `RestTemplate`은 별도의 유틸리티 클래스로 생성
- 혹은 서비스 혹은 비즈니스 계층에서 구현

```java
@Service
public class RestTemplateService {

    // 파라미터 호출 X
    public String getName() {
        URI uri = UriComponentsBuilder
            .fromUriString("http://localhost:9090")
            .path("/api/v1/crud-api")
            .encode()
            .build()
            .toUri();

        RestTemplate restTemplate = new RestTemplate();
        ResponseEntity<String> responseEntity = restTemplate.getForEntity(uri, String.class);

        return responseEntity.getBody();
    }

}
```

`UriComponentsBuilder`
- `RestTempalte`을 생성하고 사용하는 가장 보편적인 방법
- 여러 파라미터를 연결해서 URI 형식을 만드는 기능 수행
- 빌더 형식으로 객체 생성
- `fromUriString` : 호출부의 URL
- `path` : 세부 경로
- `encode` : 인코딩 문자셋을 설정(기본값 : UTF - 8)
- `build` : 빌더 생성 종료 후 `UriComponents` 타입 리턴 
- `toUri` : URI 타입으로 리턴

`getForEntity`
- 생성된 uri로 외부 API 요청에 사용
- URI와 응답 타입을 매개변수로 사용

```java
// PathVariable 사용
public String getNameWithPathVariable() {
    URI uri = UriComponentsBuilder
        .fromUriString("http://localhost:9090")
        .path("/api/v1/crud-api/{name}")
        .encode()
        .build()
        .expand("Flature")
        .toUri();

    RestTemplate restTemplate = new RestTemplate();
    ResponseEntity<String> responseEntity = restTemplate.getForEntity(uri, String.class);

    return responseEntity.getBody();
}
```

변수명 입력
- `path` : 세부 URI 중 중괄호 부분을 사용해 변수명을 입력
- `expand` : 변수에 넣을 값을 차례대로 입력(`,`로 구분)

```java
// 파라미터 사용
public String getNameWithParameter() {
    URI uri = UriComponentsBuilder
        .fromUriString("http://localhost:9090")
        .path("/api/v1/crud-api/param")
        .queryParam("name", "Flature")
        .encode()
        .build()
        .toUri();

    RestTemplate restTemplate = new RestTemplate();
    ResponseEntity<String> responseEntity = restTemplate.getForEntity(uri, String.class);

    return responseEntity.getBody();
}
```

`queryParam`
- 키, 값 형식으로 파라미터를 추가 가능

---

### 5. POST 형식 RestTemplate 작성

```java
public ResponseEntity<MemberDto> postWithParamAndBody() {
    URI uri = UriComponentsBuilder
        .fromUriString("http://localhost:9090")
        .path("/api/v1/crud-api")
        .queryParam("name", "Flature")
        .queryParam("email", "flature@wikibooks.co.kr")
        .queryParam("organization", "Wikibooks")
        .encode()
        .build()
        .toUri();

    MemberDto memberDto = new MemberDto();
    memberDto.setName("flature!!");
    memberDto.setEmail("flature@gmail.com");
    memberDto.setOrganization("Around Hub Studio");

    RestTemplate restTemplate = new RestTemplate();
    ResponseEntity<MemberDto> responseEntity = restTemplate.postForEntity(uri, memberDto,
        MemberDto.class);

    return responseEntity;
}
```

- RequestBody에 담기 위해선 데이터 객체 생성
- `postForEntity` 사용 시 파라미터로 데이터 객체를 넣어주면됨

```java
public ResponseEntity<MemberDto> postWithHeader() {
    URI uri = UriComponentsBuilder
        .fromUriString("http://localhost:9090")
        .path("/api/v1/crud-api/add-header")
        .encode()
        .build()
        .toUri();

    MemberDto memberDTO = new MemberDto();
    memberDTO.setName("flature");
    memberDTO.setEmail("flature@wikibooks.co.kr");
    memberDTO.setOrganization("Around Hub Studio");

    RequestEntity<MemberDto> requestEntity = RequestEntity
        .post(uri)
        .header("my-header", "Wikibooks API")
        .body(memberDTO);

    RestTemplate restTemplate = new RestTemplate();
    ResponseEntity<MemberDto> responseEntity = restTemplate.exchange(requestEntity,
        MemberDto.class);

    return responseEntity;
}
```

`RequestEntity`
- 요청의 각 속성을 정의 가능
- 특히 인증을 위한 토큰을 담는 `header`를 많이 사용

`exchange`
- 모든 형식의 HTTP 요청을 생성 가능
- `RequestEntity`에서 다른 형식으로 정의만 하면 `exchange`로 쉽게 사용 가능

---

### 6. RestTemplate 커스텀 설정

RestTemplate은 기본적으로 커넥션 풀을 지원 X
- 매번 호출할 때마다 포트를 열어 커넥션 생성
- TIME_WAIT 상태가 된 소켓을 다시 사용하려고 접근하면 재사용 X
- 이를 방지하기 위해 커넥션 풀 기능을 활성화하는 것이 좋음

아파치에서 제공하는 HttpClient로 대체해서 사용하는 방식이 대표적

#### 의존성 추가

Maven

```xml
<dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>httpclient</artifactId>
</dependency>
```

Gradle

```
implementation 'org.apache.httpcomponents:httpclient'
```

#### RestTemplate 객체 생성 메서드

```java
public RestTemplate restTemplate() {
    HttpComponentsClientHttpRequestFactory factory = new HttpComponentsClientHttpRequestFactory();

    HttpClient client = HttpClientBuilder.create()
        .setMaxConnTotal(500)
        .setMaxConnPerRoute(500)
        .build();

    CloseableHttpClient httpClient = HttpClients.custom()
        .setMaxConnTotal(500)
        .setMaxConnPerRoute(500)
        .build();

    factory.setHttpClient(httpClient);
    factory.setConnectTimeout(2000);
    factory.setReadTimeout(5000);

    RestTemplate restTemplate = new RestTemplate(factory);

    return restTemplate;
}
```

`HttpComponentsClientHttpRequestFactory`
- `RestTemplate`의 생성자를 보면 `ClientRequestFactory`를 매개변수로 받는 생성자가 존재
- 대표적인 구현체는 `SimpleClientHttpRequestFactory`와 `HttpComponentsClientHttpRequestFactory`
- 커넥션풀을 유지하기 위해 `HttpClient` 설정 가능

`HttpClient`
- `HttpClientBuilder.create()` 혹은 `HttpClients.custom()` 이용하여 생성
- `factory`의 `setHttpClient` 메서드를 통해 인자로 전달하여 설정

---
<br>

## Ⅱ. WebClient

> Spring WebFlux에서 제공하는 Http 요청 수행 클라이언트

- 리액터 기반으로 동작하는 API
- 리액터 기반이므로 스레드와 동시성 문제를 벗어나 비동기 형식 사용 가능

### 1. WebClient의 특징

- 논블로킹을 지원
- 리액티브 스트림의 백 프레셔 지원
- 적은 하드웨어 리소스로 동시성 지원
- 함수형 API 지원
- 동기, 비동기 상호작용을 지원
- 스트리밍을 지원

---

### 2. WebClient 의존성 주입

Maven

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-webflux</artifactId>
    </dependency>
</dependencies>
```

Gradle

```
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-webflux'
}
```

---

### 3. GET 요청 WebClient 구현

`WebClient`는 우선 객체를 생성한 후 요청을 전달하는 방식으로 동작

```java
@Service
public class WebClientService {

    public String getName() {
        WebClient webClient = WebClient.builder()
            .baseUrl("http://localhost:9090")
            .defaultHeader(HttpHeaders.CONTENT_TYPE, MediaType.APPLICATION_JSON_VALUE)
            .build();

        return webClient.get()
            .uri("/api/v1/crud-api")
            .retrieve()
            .bodyToMono(String.class)
            .block();
    }

}
```

이 함수는 `builder`를 통해 객체 생성
- `baseUrl` : 기본 URL을 설정
- `defaultHeader` : 헤더의 값을 설정

이 외에도 `builder` 사용 시 다양한 메서드 사용하여 확장 가능
- `defaultHeader` : `WebClient`의 기본 헤더 설정
- `defaultCookie` : `WebClient`의 기본 쿠키 설정
- `defaultUriVariable` : `WebClient`의 기본 URI 확장값 설정
- `filter` : `WebClient`에서 발생하는 요청에 대한 필터 설정

이러한 `WebClient` 객체 생성 후 재사용하는 방식으로 구현하는 것이 좋음
- 빌드된 `WebClient`는 변경할 수 없음
- `webClient.mutate().build()` : `WebClient` 객체를 복사

아래는 반환값 설정 코드 부분 설명이다.
- `.get()` : GET 방식 요청
- `.uri()` : URI 확장
- `.retrieve()` : 요청에 대한 응답을 받았을 때 값을 추출하는 방법 중 하나
- `.bodyToMono(String.class)` : `retrieve`가 받는 데이터를 문자열로 설정
- `.block()` : 기본적으로 논블로킹 방식으로 동작하는 것을 블로킹 형식으로 변경

```java
public String getNameWithPathVariable() {
    WebClient webClient = WebClient.create("http://localhost:9090");

    ResponseEntity<String> responseEntity = webClient.get()
        .uri(uriBuilder -> uriBuilder.path("/api/v1/crud-api/{name}")
            .build("Flature"))
        .retrieve().toEntity(String.class).block();

    ResponseEntity<String> responseEntity1 = webClient.get()
        .uri("/api/v1/crud-api/{name}", "Flature")
        .retrieve()
        .toEntity(String.class)
        .block();

    return responseEntity.getBody();
}
```

`PathVariable` 값을 추가해 요청을 보내는 코드
- `uri` 메서드 내부에 `uriBuilder`를 사용해 `path`를 설정
- `build` 메서드에 추가할 값을 넣어서 `PathVariable` 추가

`toEntity`
- `ResponseEntity` 타입으로 응답을 전달받을 수 있음

```java
public String getNameWithParameter() {
    WebClient webClient = WebClient.create("http://localhost:9090");

    return webClient.get().uri(uriBuilder -> uriBuilder.path("/api/v1/crud-api")
            .queryParam("name", "Flature")
            .build())
        .exchangeToMono(clientResponse -> {
            if (clientResponse.statusCode().equals(HttpStatus.OK)) {
                return clientResponse.bodyToMono(String.class);
            } else {
                return clientResponse.createException().flatMap(Mono::error);
            }
        })
        .block();
}
```

쿼리 파라미터를 함께 전달
- `uriBuilder` 사용
- `queryParam` 사용

`exchangeToMono`
- `exchange` 메서드가 지원 종료됨에 따라 대신 사용
- 응답 결과 코드에 따라 다르게 응답 가능

---

### 4. POST 요청 WebClient 구현

```java
public ResponseEntity<MemberDto> postWithParamAndBody() {
    WebClient webClient = WebClient.builder()
        .baseUrl("http://localhost:9090")
        .defaultHeader(HttpHeaders.CONTENT_TYPE, MediaType.APPLICATION_JSON_VALUE)
        .build();

    MemberDto memberDTO = new MemberDto();
    memberDTO.setName("flature!!");
    memberDTO.setEmail("flature@gmail.com");
    memberDTO.setOrganization("Around Hub Studio");

    return webClient.post().uri(uriBuilder -> uriBuilder.path("/api/v1/crud-api")
            .queryParam("name", "Flature")
            .queryParam("email", "flature@wikibooks.co.kr")
            .queryParam("organization", "Wikibooks")
            .build())
        .bodyValue(memberDTO)
        .retrieve()
        .toEntity(MemberDto.class)
        .block();
}

public ResponseEntity<MemberDto> postWithHeader() {
    WebClient webClient = WebClient.builder()
        .baseUrl("http://localhost:9090")
        .defaultHeader(HttpHeaders.CONTENT_TYPE, MediaType.APPLICATION_JSON_VALUE)
        .build();

    MemberDto memberDTO = new MemberDto();
    memberDTO.setName("flature!!");
    memberDTO.setEmail("flature@gmail.com");
    memberDTO.setOrganization("Around Hub Studio");

    return webClient
        .post()
        .uri(uriBuilder -> uriBuilder.path("/api/v1/crud-api/add-header")
            .build())
        .bodyValue(memberDTO)
        .header("my-header", "Wikibooks API")
        .retrieve()
        .toEntity(MemberDto.class)
        .block();
}
```

`bodyValue`
- HTTP 바디 값을 설정
- 일반적으로 데이터객체를 파라미터로 전달

`header`
- POST 요청시 header 값을 추가










