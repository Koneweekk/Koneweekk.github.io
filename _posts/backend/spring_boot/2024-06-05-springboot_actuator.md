---
title: Spring Boot - 액추에이터(Actuator)
date: 2024-06-05 10:50:00 +09:00
categories: [Back-End, Spring Boot]
tags: [Back-End, Java, Spring Boot, Actuator]
---

## Ⅰ. 액추에이터(Actuator)란?

### 1. 액추에이터(Actuator)란

> Spring Boot 애플리케이션의 모니터링 및 관리 기능을 제공하는 모듈

엔드포인트(Endpoints)
- 다양한 정보를 제공하는 여러 엔드포인트를 통해 애플리케이션의 상태를 모니터링
  
모니터링
- Micrometer를 통해 다양한 모니터링 시스템과 연동 가능
- Prometheus, Graphite, DataDog, New Relic, 그리고 JMX 등과 통합 가능

---

### 2. JMX란

> JMX(Java Management Extensions) : Java 애플리케이션을 관리하고 모니터링하기 위한 프레임워크

- 다양한 자원을 관리할 수 있는 표준 방법을 제공
- 애플리케이션의 상태를 실시간으로 모니터링하고 제어할 수 있도록 도움

---
<br>

## Ⅱ. 엑추에이터 초기 설정

### 1. 의존성 주입

#### Maven

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

#### Gradle

```
implementation 'org.springframework.boot:spring-boot-starter-actuator'
```

---

### 2. 엔드포인트 설정

엑추에이터의 엔드포인트
- 애플리케이션의 모니터링을 사용하는 경로
- 스프링부트 내장 엔드포인트들이 포함
- 커스텀 엔드포인트 추가 가능
- 기본 경로로 `/actuator` 가 추가되며 경로를 추가해 상세내역 접근

기본 경로를 변경하고 싶다면 다음과 같이 application.properties 파일 작성
`management.endpoints.web.base-path=/custom-path`

자주 활용되는 엑추에이터 엔드포인트는 다음과 같다

|엔드포인트|설명|
|---|---|
|`auditevents`|최근 감사 이벤트를 보여준다.|
|`beans`|애플리케이션 컨텍스트 내에서 정의된 모든 스프링 빈을 보여준다.|
|`caches`|캐시 관련 정보를 보여준다.|
|`conditions`|자동 구성 조건에 대한 보고서를 보여준다.|
|`configprops`|모든 @ConfigurationProperties 빈에 대한 정보를 보여준다.|
|`env`|애플리케이션의 환경 변수와 프로파일 정보를 보여준다.|
|`flyway`|Flyway 데이터베이스 마이그레이션 상태를 보여준다.|
|`health`|애플리케이션의 상태를 보여준다.|
|`httptrace`|최근 HTTP 요청과 응답을 보여준다.|
|`info`|애플리케이션에 대한 임의의 정보를 보여준다.|
|`integrationgraph`|스프링 통합 그래프를 표시. spring-integration-core 모듈에 대한 의존성 추가 필요|
|`liquibase`|Liquibase 데이터베이스 마이그레이션 상태를 보여준다.|
|`loggers`|애플리케이션의 로깅 설정을 보여주고 변경할 수 있다.|
|`mappings`|애플리케이션의 모든 @RequestMapping 경로를 보여준다.|
|`metrics`|애플리케이션의 메트릭 정보를 보여준다.|
|`quartz`|Quartz 스케줄러 작업에 대한 정보 표시|
|`session`|스프링 세션 저장소에서 사용자의 세션을 검색하고 삭제 가능|
|`shutdown`|애플리케이션을 정상적으로 종료 가능|
|`startup`|애플리케이션이 시작될 때 수집 된 시작 단계 데이터 표시|
|`scheduledtasks`|스케줄된 작업에 대한 정보를 보여준다.|
|`threaddump`|JVM의 스레드 덤프를 보여준다.|

Spring MVC, Spring WebFlux에서 추가로 사용할 수 있는 엔드 포인트는 다음과 같다.

|엔드포인트|설명|
|---|---|
|`heapdump`|힙 덤프 파일을 반환|
|`jolokia`|Jolokia가 클래스패스에 있을 때 HTTP를 통해 JMX를 표시|
|`logfile`|logging.file.name 또는 logging.file.path 속성이 설정시 로그 파일 내용을 반환|
|`Prometheus`|Prometheus 서버에서 스크랩할 수 있는 형식으로 매트릭 표시|

엔드 포인트는 활서오하 여부와 노출 여부를 설정할 수 있다.
- 활성화 : 기능 자체를 활성화할 것인지를 결정
- 비활성화 : 애플리케이션 컨텍스트에서 완전히 제거

```
management.endpoint.shutdown.enabled=true
management.endpoint.caches.enabled=false
```

엔드 포인트의 노출 여부만 설정하는 것도 가능
- 노출 여부는 JMX를 통한 노출과 HTTP를 통한 노출이 존재

```
## HTTP 설정
management.endpoints.web.exposure.include=*
management.endpoints.web.exposure.exclude=threaddump,heapdump
## JMX 설정
management.endpoints.jmx.exposure.include=*
management.endpoints.jmx.exposure.exclude=threaddump, heapdump
```

엔드포인트 노출의 기본값 설정은 다음과 같다

|엔드포인트|JMX|WEB|
|---|---|---|
|`auditevents`      |O|X|
|`beans`            |O|X|
|`caches`           |O|X|
|`conditions`       |O|X|
|`configprops`      |O|X|
|`env`              |O|X|
|`flyway`           |O|X|
|`health`           |O|O|
|`httptrace`        |O|X|
|`info`             |O|X|
|`integrationgraph` |O|X|
|`liquibase`        |O|X|
|`loggers`          |O|X|
|`mappings`         |O|X|
|`metrics`          |O|X|
|`quartz`           |O|X|
|`session`          |O|X|
|`shutdown`         |O|X|
|`startup`          |O|X|
|`scheduledtasks`   |O|X|
|`threaddump`       |O|X|
|`heapdump`         |해당없음|X|
|`jolokia`          |해당없음|O|
|`logfile`          |해당없음|O|
|`prometheus`       |해당없음||

---
<br>

## Ⅲ. 액츄에이터 기능

### 1. 애플리케이션 기본 정보(/info)

application 파일에 info 속성을 정의하는 것이 가장 쉬운 방법

```
info.organization.name=koneweekk
info.contact.email=koneweekk@gmail.com
info.contact.phoneNumber=010-1234-5678
```

그 후 서버 가동 후 브라우저에 아래 url로 접근하면 다음과 같은 결과값을 얻을 수 있다.
- http://localhost:{설정한 포트 번호}/actuator/info

```json
{
  "organization": {
    "name": "wikibooks"
  },
  "contact": {
    "email": "thinkground.flature@gmail.com",
    "phoneNumber": "010-1234-5678"
  },
}
```

---

### 2. 애플리케이션 상태(/health)

별도의 설정없이 다음 url에 접근하면 다음과 같은 결과를 얻을 수 있다.
- http://localhost:8090/actuator/health

```json
{"status": "UP"}
```
- 주로 네트워크 L4(Loadbalancing) 레벨에서 애플리케이션 상태 확인을 위해 사용됨
- 얻을 수 있는 상대 지표 : UP, DOWN, UNKNOWN, OUT_OF_SERVICE

상세 상태 확인은 application.properties에 다음과 같은 설정이 필요하다

```
management.endpoint.health.show-details=always
```

설정 변경 후 결과는 다음과 같다

```json
{
  "status": "UP",
  "components": {
    "diskSpace": {
      "status": "UP",
      "details": {
        "total": 233981341696,
        "free": 123224592384,
        "threshold": 10485760,
        "exists": true
      }
    },
    "ping": {
      "status": "UP"
    }
  }
}
```

- 만약 DB가 연동되어 있다면 인프라 관련 상태까지 확인 가능
- 모든 `status`가 `UP`이어야 애플리케이션 상태가 `UP`이 됨

---

### 3. 빈 정보 확인(/beans)

스프링 컨테이너에 등록된 스프링 빈의 전체 목록을 표시
- http://localhost:8090/actuator/beans

수많은 빈이 자동으로 등록
- 육안으로 내용을 파악하기는 어려움

---

### 4. 스프링 부트의 자동 설정 내역 확인(/conditions)

스프링부트의 자동설정 조건 내역을 확인
- http://localhost:8090/actuator/conditions

출력내용은 크게 `positiveMatches`와 `negativeMatches` 나뉨
- 자동설정의 `@Conditional`에 따라 평가된 내용을 표시

---

### 5. 스프링 환경변수 정보 (/env)

스프링의 환경변수 정보를 확인하는데 사용
- http://localhost:8090/actuator/env

기본적으로 application.properties의 변수들이 표시
- OS.JVM의 환경변수도 함께 표시

일부 민감한 정보는 표시하지 않을 수 있음
- management.endpoint.env.keys-to-sanitize 속성 사용
- 단순 문자열 혹은 정규식도 사용 가능
  
---

### 6. 로깅 레벨 확인(/loggers)

애플리케이션의 로깅 레벨 수준 확인 가능
- http://localhost:8090/actuator/loggers

POST로 호출하면 로깅 레벨을 변경하는 것도 가능

---
<br>

## Ⅳ. 엑추에이터 커스텀 기능

### 1. 정보 제공 인터페이스의 구현체 생성

별도의 구현체 클래스를 작성하는 방법이 엑추에이터 커스터마이징에 많이 활용됨
application.properties 파일 내에 내용을 추가는 간단하나 많은 내용을 담기 어려움

```java
@Component
public class CustomInfoContributor implements InfoContributor {

    @Override
    public void contribute(Builder builder) {
        Map<String, Object> content = new HashMap<>();
        content.put("code-info", "InfoContributor 구현체에서 정의한 정보입니다.");
        builder.withDetail("custom-info-contributor", content);
    }
}
```

`contribute`
- 구현하고있는 `InfoContributor`의 메서드를 오버라이드

`Builder`
- 엑추에이터 패키지의 `Info` 클래스 안에 정의돼 잇는 클래스
- `Info` 엔드포인트에서 보여줄 내용을 담는 역할을 수행

```json
{
  "organization": {
    "name": "wikibooks"
  },
  "contact": {
    "email": "thinkground.flature@gmail.com",
    "phoneNumber": "010-1234-5678"
  },
  "custom-info-contributor": {
    "code-info": "InfoContributor 구현체에서 정의한 정보입니다."
  }
}
```

http://localhost:8090/actuator/info에 접근하면 `custom-info-contributor`가 추가된 것을 확인 가능

---

### 2. 커스텀 엔드포인트 생성

```java
@Component
@Endpoint(id = "note", enableByDefault = true)
public class NoteEndpoint {

    private Map<String, Object> noteContent = new HashMap<>();

    @ReadOperation
    public Map<String, Object> getNote(){
        return noteContent;
    }

    @WriteOperation
    public Map<String, Object> writeNote(String key, Object value){
        noteContent.put(key,value);
        return noteContent;
    }

    @DeleteOperation
    public Map<String, Object> deleteNote(String key){
        noteContent.remove(key);
        return noteContent;
    }

}
```

`@Endpoint`
- 액추에이터에 엔드포인트로 자동 들록
- `id` : 경로를 정의 가능
- `enableByDefault` : 현재 생성하는 엔드포인트 기본 환성화 여부 설정 가능(기본값 true)

`@ReadOperation`, `@WriteOperation`, `@DeleteOperation`을 통해 엔드포인트 노출 가능
- 각 어노테이션은 GET, POST, DELETE 메서드에 대응

`@JmxEndpoint`, `@WebEndpoint`를 통해 JMX 혹은 Http에 노출 제한