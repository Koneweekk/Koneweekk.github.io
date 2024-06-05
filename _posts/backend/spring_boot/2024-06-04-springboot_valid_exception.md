---
title: Spring Boot - 유효성 검사 및 예외 처리
date: 2024-06-04 11:50:00 +09:00
categories: [Back-End, Spring Boot]
tags: [Back-End, Java, Spring Boot, Validation, Exception]
---

## Ⅰ. 유효성 검사

### 1. 의존성 추가

spring Boot는 Bean Validation API와 Hibernate Validator를 사용하여 유효성 검사를 지원

아래와 같이 build.gradle에 의존성 주입

```
dependencies {
  ...
  implementation 'org.springframework.boot:spring-boot-starter-validation'
  ...
}
```

---

### 2. 스프링 부트의 유효성 검사

각 계층으로 데이터가 넘어오는 시점에 해당 데이터에 대한 검사를 실시
- 주로 DTO 객체개 대상
- 계층 간 데이터 전송에 대체로 DTO 객체를 활용하기 때문

유효성 검증을 실시할 DTO를 다음과 같이 정의

#### 유효성 검증 어노테이션을 적용한 DTO

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
@ToString
@Builder
public class ValidRequestDto {

    @NotBlank
    private String name;

    @Email
    private String email;

    @Pattern(regexp = "01(?:0|1|[6-9])[.-]?(\\d{3}|\\d{4})[.-]?(\\d{4})$")
    private String phoneNumber;

    @Min(value = 20)
    @Max(value = 40)
    private int age;

    @Size(min = 0, max = 40)
    private String description;

    @Positive
    private int count;

    @AssertTrue
    private boolean booleanCheck;

}
```

문자열 검증
- `@Null` : Null 값만 허용
- `@NotNull` : Null을 허용하지 않음. "", " "는 허용
- `@NotEmpty` : null, ""을 허용하지 않음. " "는 허용
- `@NotBlank` : null, "", " " 모두 허용하지 않음

최대값/최솟값
- `DecimalMin(value = "$numberString")` : $numberString 이상의 값을 허용
- `DecimalMax(value = "$numberString")` : $numberString 이하의 값을 허용
- `@Min(value = $number)` : $number 이상의 값을 허용
- `@Max(value = $number)` : $number 이하의 값을 허용

값의 범위 검증
- `@Positive` : 양수를 허용
- `@PositiveOrZero` : 0을 포함한 양수를 허용
- `@Negative` : 음수를 허용
- `@NegativeOrZero` : 0을 포함한 음수를 허용

시간에 대한 검증
`@Future` : 현재보다 미래의 날짜를 허용
`@FutureOrPresent` : 현재를 포함한 미래의 날짜를 허용
`@Past` : 현재보다 과거의 날짜를 허용
`@PastOrPresent` : 현재를 포함한 과거의 날짜를 허용

이메일 검증
`@Email` : 이메일 형식을 검사. ""는 허용

자릿수 범위 검증
`@Digits(integer = $number1, fraction = $number2)` : $number1의 정수 자릿수와 $number2의 소수 자릿수 허용

Boolean 검증
- `@AssertTrue` : true 체크, null 값은 체크하지 않음
- `@AssertFalse` : false 체크, null 값은 체크하지 않음

문자열 길이 검증
- `@Size(min = $minNumber, max = $maxNumber)` : 문자열의 길이를 제한

정규식 검증
- `@Pattern(regexp = expression)` : 정규식을 검사


#### Controller에서의 검증

```java
@RestController
@RequestMapping("/validation")
public class ValidationController {

    private final Logger LOGGER = LoggerFactory.getLogger(ValidationController.class);

    @PostMapping("/valid")
    public ResponseEntity<String> checkValidationByValid(@Valid @RequestBody ValidRequestDto validRequestDto) {
        LOGGER.info(validRequestDto.toString());
        return ResponseEntity.status(HttpStatus.OK).body(validRequestDto.toString());
    }

}
```

`@Valid`
- DTO 객체애 대해 유효성 검사를 수행
- `@RequestBody`를 통해 매개변수를 입력받을 경우 유효섬 검증을 위해 넣어줘야함

위 컨트롤러를 SWAGGER를 통해 검증해보자

![swagger](/assets/img/post/backend/spring-boot/2024-06-04-springboot-valid_exception/01.png)

DTO 입력값이 유효하면 다음과 같이 '200 OK'로 응답

![swagger](/assets/img/post/backend/spring-boot/2024-06-04-springboot-valid_exception/02.png)

유효하지않다면 400에러 발생
- 유효성을 통과하지 못한 필드에 대한 정보 제공
- 2개 이상의 필드가 통과하지 못하면 그 개수도 로그에 남음

![swagger](/assets/img/post/backend/spring-boot/2024-06-04-springboot-valid_exception/03.png)

---

### 3. @Validated

> 유효성 검사를 위해 스프링에서 지원하는 어노테이션 

- `@valid` 어노테이션 기능 포함
- 유효성 검사를 그룹으로 묶어 대상을 특정할 수 있음
- 검증 그룹은 마커 인터페이스(오직 그룹화에만 사용)를 생성해서 사용

우선 그룹화를 위해 마커를 생성

```java
package com.springboot.valid_exception.data.group;

public interface ValidationGroup1 {

}
```

```java
package com.springboot.valid_exception.data.group;

public interface ValidationGroup2 {

}
```

검증 그룹 설정은 DTO객체에서 실시

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
@ToString
@Builder
public class ValidatedRequestDto {

    @NotBlank
    private String name;

    @Email
    private String email;

    @Pattern(regexp = "01(?:0|1|[6-9])[.-]?(\\d{3}|\\d{4})[.-]?(\\d{4})$")
    private String phoneNumber;

    @Min(value = 20, groups = ValidationGroup1.class)
    @Max(value = 40, groups = ValidationGroup1.class)
    private int age;

    @Size(min = 0, max = 40)
    private String description;

    @Positive(groups = ValidationGroup2.class)
    private int count;

    @AssertTrue
    private boolean booleanCheck;

}
```

`groups = ValidationGroup1.class`
- 어느 그룹에 맞춰 유효성 검사를 실시할 것인지 지정

실제로 그룹을 어떻게 설정해서 유효성 검사를 실시할 것인지는 `@Validated`에서 결정

```java
@RestController
@RequestMapping("/validation")
public class ValidationController {

    private final Logger LOGGER = LoggerFactory.getLogger(ValidationController.class);

    @PostMapping("/validated")
    public ResponseEntity<String> checkValidation(
            @Validated @RequestBody ValidatedRequestDto validatedRequestDto) {
        LOGGER.info(validatedRequestDto.toString());
        return ResponseEntity.status(HttpStatus.OK).body(validatedRequestDto.toString());
    }

    @PostMapping("/validated/group1")
    public ResponseEntity<String> checkValidation1(
            @Validated(ValidationGroup1.class) @RequestBody ValidatedRequestDto validatedRequestDto) {
        LOGGER.info(validatedRequestDto.toString());
        return ResponseEntity.status(HttpStatus.OK).body(validatedRequestDto.toString());
    }

    @PostMapping("/validated/all-group")
    public ResponseEntity<String> checkValidation2(
            @Validated({ValidationGroup1.class,
                    ValidationGroup2.class}) @RequestBody ValidatedRequestDto validatedRequestDto) {
        LOGGER.info(validatedRequestDto.toString());
        return ResponseEntity.status(HttpStatus.OK).body(validatedRequestDto.toString());
    }
}
```

`@Validated()`
- 특정 그룹을 지정하지 않는 경우 `groups` 속성을 설정하지 않은 필드에 대해서만 유효성 검사 실시
- 특정 그룹을 설정하는 경우 지정된 그룹으로 설정된 필드에 대해서만 유효성 검사 실시

---

### 4. 커스템 Validation 추가

`ConstraintValidator`와 커스텀 어노테이션을 조합해 별도의 유효성 검사 어노테이션 생성 가능

`ConstraintValidator` 인터페이스를 구현하는 클래스는 다음과 같이 생성
- 어떤 어노테이션 인터페이스인지 타입을 지정해야함

```java
public class TelephoneValidator implements ConstraintValidator<Telephone, String> {

    @Override
    public boolean isValid(String value, ConstraintValidatorContext context) {
        if(value==null){
            return false;
        }
        return value.matches("01(?:0|1|[6-9])[.-]?(\\d{3}|\\d{4})[.-]?(\\d{4})$");
    }
}
```

`isValid`
- 이 메서드를 오버라이드하여 직접 유효성 검사 로직 작성
- 이 로직에서 false가 리턴 되면 `MethodArgumentNovalidExceotion`이 발생

`Telephone` 어노테이션은 다음과 같이 선언할 수 있다.

```java
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy = TelephoneValidator.class)
public @interface Telephone {
    String message() default "전화번호 형식이 일치하지 않습니다.";
    Class[] groups() default {};
    Class[] payload() default {};
}
```

`message()` : 유효성 검사가 실패한 경우 반환되는 메시지를 의미
`groups()` : 유효성 검사를 사용하는 그룹으로 설정
`payload()` : 사용자가 추가 정보를 위해 전달하는 값

`@Target`
- 어디서 어노테이션이 사용될 수 있는지 정의
- `ElementType`을 통해 지정

`@Retention`
- 어노테이션이 실제로 적용되고 유지되는 범위
- `RetentionPolicy`을 통해 지정

`@Constrain`
- 위에서 정의한 `TelephoneValidator`와 매핑

위에서 정의한 `@Telephone` 어노테이션을 다음과 같이 사용

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
@ToString
@Builder
public class ValidatedRequestDto {

    @NotBlank
    private String name;

    @Email
    private String email;

    @Telephone(groups = ValidationGroup2.class)
    private String phoneNumber;

    @Min(value = 20, groups = ValidationGroup1.class)
    @Max(value = 40, groups = ValidationGroup1.class)
    private int age;

    @Size(min = 0, max = 40)
    private String description;

    @Positive(groups = ValidationGroup2.class)
    private int count;

    @AssertTrue
    private boolean booleanCheck;

}
```

---
<br>

## Ⅱ. 예외 처리

### 1. 예외와 에러

#### 예외
- 입력 값의 처리가 불가능하는 등 애플리케이션이 정상 작동하지 못하는 경우
- 예외는 개발자가 직접 처리할 수 있는 것이므로 코드 설계를 통해 처리 가능

#### 에러
- 주로 자바의 가상머신에서 발생시키는 것
- 예외와 달리 애플리케이션 코드에서 처리할 수 있는 것이 없음
- 대표적으로 StackOverFlow 혹은 OutOfMemory 등이 있다.
- 미리 애플리케이션 코드를 살펴보며 문제가 발생하지 않도록 예방해얗마

---

### 2. 예외 클래스

자바의 예외 클래스는 다음과 같은 상속 구조를 갖추고 있다.

![예외 클래스](https://www.tcpschool.com/lectures/img_java_exception_class_hierarchy.png)

- 모든 예외 클래스는 `Throwable` 클래스 상속
- `Exception` 클래스는 다양한 자식 클래스를 가짐
- 크게 Checked Exception과 Unchecked Exception으로 나뉨

#### Checked Exception

> 컴파일 단계에서 확인 가능한 예외 상황

- 반드시 예외 처리 필요
- `IOExcetion`, `SQLException` 등이 대표적

#### Unchecked Exception

> 런타임 단계에서 확인되는 예외 상황

- 명시적 처리를 강제하지 않음
- `RuntimeException`을 상속받음
- `IndexOutOfBoundException`, `NullPointerException` 등이 대표적

---

### 3. 예외 처리 방법

#### 예외 복구

대표적으로 `try/catch`문을 사용
- `try` : 예외 발생 가능한 구문 작성
- `catch` : `try`에서 발생한 예외 상황 처리

```java
int a = 1;
String b = "a"

try {
  System.out.println(a + Integer.parseInt(b));
} catch(NumberFormatException e) {
  b = "2";
  System.out.println(a + Integer.parseInt(b));
}
```

#### 예외 처리 회피

대표적으로 `throw` 키워드 사용
- 예외가 발생한 메서드를 호출한 곳에 에러 처리를 전가

```java
int a = 1;
String b = "a"

try {
  System.out.println(a + Integer.parseInt(b));
} catch(NumberFormatException e) {
  throw new NumberFormatException("숫자가 아닙니다.");
}
```

---

### 4. 스프링 부트의 예외 처리

스프링에선 클라이언트에 어떤 문제를 발생했는지 상황을 전달하는 경우가 많음
- 예외가 발생했을 때 클라이언트에 오류 메시지를 전달하려면 엔드포인트 레벨인 컨트롤러로 전달해야함

스프링부트는 보통 아래 두 가지 방법으로 예외를 처리
- `@(Rest)ContollerAdvice`와 `@ExceptionHandler`를 통해 모든 컨트롤러의 예외 처리
- `@ExceptionHandler`를 통해 특정 컨트롤러의 예외 처리

```java
@RestControllerAdvice
public class CustomExceptionHandler {

    private final Logger LOGGER = LoggerFactory.getLogger(CustomExceptionHandler.class);

    @ExceptionHandler(value = RuntimeException.class)
    public ResponseEntity<Map<String, String>> handleException(RuntimeException e,
        HttpServletRequest request) {
        HttpHeaders responseHeaders = new HttpHeaders();
        HttpStatus httpStatus = HttpStatus.BAD_REQUEST;

        LOGGER.error("Advice 내 exceptionHandler 호출, {}, {}", request.getRequestURI(),
            e.getMessage());

        Map<String, String> map = new HashMap<>();
        map.put("error type", httpStatus.getReasonPhrase());
        map.put("code", "400");
        map.put("message", e.getMessage());

        return new ResponseEntity<>(map, responseHeaders, httpStatus);
    }

}
```

`@RestControllerAdvice`
- `@Controller` 혹은 `@RestContoller`에서 발생하는 예외를 한 곳에서 관리하고 처리
- `basePackages` 속성으로 예외를 관제하는 범위 지정 가능(ex : `basePackages = "com.springboot.valid_exception"`)

`@ExceptionHandler`
- 컨트롤러에서 발생하는 예외를 잡아 처리하는 메서드를 정의할 때 사용
- `value` 속성으로 처리할 예외 클래스등록(배열 형태로 다수의 클래스 등록 가능)

![@RestControllerAdvice](/assets/img/post/backend/spring-boot/2024-06-04-springboot-valid_exception/04.png)

SWAGGER를 통해 runtime 에러를 발생시켜보면 위 그림 처럼 응답이 오는 것을 확인할 수 있다.
- 컨트롤러에서 던진 예외는 ControllerAdvice가 선언된 핸드러 클래스에서 매핑된 예외 타입을 찾아 처리

```java
@RestController
@RequestMapping("/exception")
public class ExceptionController {

    private final Logger LOGGER = LoggerFactory.getLogger(ExceptionController.class);

    @GetMapping
    public void getRuntimeException() {
        throw new RuntimeException("getRuntimeException 메소드 호출");
    }

    @ExceptionHandler(value = RuntimeException.class)
    public ResponseEntity<Map<String, String>> handleException(RuntimeException e,
        HttpServletRequest request) {
        HttpHeaders responseHeaders = new HttpHeaders();
        responseHeaders.setContentType(MediaType.APPLICATION_JSON);
        HttpStatus httpStatus = HttpStatus.BAD_REQUEST;

        LOGGER.error("클래스 내 handleException 호출, {}, {}", request.getRequestURI(),
            e.getMessage());

        Map<String, String> map = new HashMap<>();
        map.put("error type", httpStatus.getReasonPhrase());
        map.put("code", "400");
        map.put("message", e.getMessage());

        return new ResponseEntity<>(map, responseHeaders, httpStatus);
    }

}
```

위 코드처럼 컨트롤러 안 메서드에 `@ExceptionHandler`를 사용 가능
- 해당 클래스에 국한해서 예외 처리 가능
- 동일한 예외 클래스 처리하는 `@ControllerAdvice`보다 우선 순위가 높음
- 만약 `@ControllerAdvice`가 더 좁은 범위의 클래스를 처리한다면 우선 순위가 낮음

---

### 5. 커스텀 예외

#### 커스텀 예외를 사용하는 이유
- 네이밍에 개발자의 의도를 담을 수 있음
- 발생하는 예외를 개발자가 직접 관리하기가 수월해짐
- 예외 상황에 대한 처리도 용이해짐

#### 커스템 예외 생성

우선 도메인 레벨 표현을 위한 열거형을 생성
- 상수들을 통합 관리하는 클래스를 생성 후 내부에 `ExceptionClass` 선언
- 커스텀 예외 클래스의 매시지에 어떤 도메인에서 문제가 발생했는지 부여주는데 사용

```java
public class Constants {

    public enum ExceptionClass {

        PRODUCT("Product");

        private String exceptionClass;

        ExceptionClass(String exceptionClass) {
            this.exceptionClass = exceptionClass;
        }

        public String getExceptionClass() {
            return exceptionClass;
        }

        @Override
        public String toString() {
            return getExceptionClass() + " Exception. ";
        }

    }

}
```

커스텀 예외 클래스를 생성하는데 필요한 내용은 다음과 같다
- 에러 타입 : `HttpStatus`의 `reasonPhrase`
- 에러 코드 : `HttpStatus`의 `value`
- 메시지 : 상황별 

```java
public class CustomException extends Exception{

    private static final long serialVersionUID = 4300333310379239987L;

    private Constants.ExceptionClass exceptionClass;
    private HttpStatus httpStatus;

    public CustomException(Constants.ExceptionClass exceptionClass, HttpStatus httpStatus,
        String message) {
        super(exceptionClass.toString() + message);
        this.exceptionClass = exceptionClass;
        this.httpStatus = httpStatus;
    }

    public Constants.ExceptionClass getExceptionClass() {
        return exceptionClass;
    }

    public int getHttpStatusCode() {
        return httpStatus.value();
    }

    public String getHttpStatusType() {
        return httpStatus.getReasonPhrase();
    }

    public HttpStatus getHttpStatus() {
        return httpStatus;
    }

}
```

`exceptionClass`과 `httpStatus` 이 두 가지 필드를 가짐
- 기존 핸들러 메서드와 달리 예외 발생 시점에 `httpStatus`를 정의해서 전달
- 클라이언트 요청에 따라 유동적인 응답 코드를 설정할 수 있음

아래 `CustomException`을 던지는 메서드를 실행해보면 예외 발생 시점이 잘 전달되는 것을 확인 가능

```java
@GetMapping("/custom")
public void getCustomException() throws CustomException {
    throw new CustomException(ExceptionClass.PRODUCT, 
                            HttpStatus.BAD_REQUEST, 
                            "getCustomException 메소드 호출"
    );
}
```

![@RestControllerAdvice](/assets/img/post/backend/spring-boot/2024-06-04-springboot-valid_exception/05.png)

