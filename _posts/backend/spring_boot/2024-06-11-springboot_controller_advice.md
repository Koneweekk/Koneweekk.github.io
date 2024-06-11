---
title: Spring Boot - Contoller Advice를 활용한 예외 처리
date: 2024-06-11 10:40:00 +09:00
categories: [Back-End, Spring Boot]
tags: [Back-End, Java, Spring Boot, Contoller Advice]
---

## Ⅰ. Contoller Advice란?

> 애플리케이션의 전역적인 예외 처리를 관리하는 데 사용되는 어노테이션

### 1. 사용하는 이유

- 컨트롤러에 한정되지 않고,애플리케이션의 모든 컨트롤러에서 발생하는 예외를 중앙에서 처리 가능
- 코드의 중복을 줄이고, 예외 처리 로직을 한 곳에 모아 관리 가능

---

### 2. 특징

#### 중앙 집중화된 예외 처리예외 
- 처리 로직을 한 곳에 모아두면 유지보수가 용이
- 코드의 일관성을 유지할 가능

#### 응답 형식 통일
- 애플리케이션 전반에 걸쳐 일관된 형식의 오류 응답을 보낼 수 있음
- 이는 클라이언트가 오류를 일관되게 처리할 수 있게 해줍니다.

#### 유연성
- 특정 예외에 대해 다양한 방식으로 처리하기 편함
- 예를 들어, 다른 HTTP 상태 코드나 메시지를 반환 가능

---
<br>

## Ⅱ. Contoller Advice 활용하기

### 1. 커스텀 예외 생성

찾으려는 데이터가 없을 시 던질 커스텀 예외 생성

```java
public class NotFoundException extends RuntimeException {

  public NotFoundException() {
    super("존재하지 않는 데이터입니다.");
  }

  public NotFoundException(String message) {
    super(message);
  }
}
```

중복된 id를 가진 데이터를 추가하려면 던질 커스텀 예외 생성

```java
public class DuplicateDataException extends RuntimeException {

  public DuplicateDataException() {
    super("이미 존재하는 데이터입니다.");
  }

  public DuplicateDataException(String message) {
    super(message);
  }
}
```

Service 레이어에서 Repository 사용 시 예외를 던짐

```java
@Service
@RequiredArgsConstructor
public class DeptService {
  
  private final DeptRepository deptRepository;

  ...

  // Read
  public DeptDto getDept(Long deptno){

    // 찾으려는 ID값을 가진 데이터가 없으면 예외 던짐
    Dept dept = deptRepository.findById(deptno).orElseThrow(() -> new NotFoundException());
    DeptDto deptDto = dept.toDto();

    return deptDto;
  }

  // Create
  public DeptDto createDept(DeptDto deptDto) {

    boolean isExist = deptRepository.findById(deptDto.getDeptno()).isPresent();
    // 추가하려는 데이터가 있으면 예외 던짐
    if (isExist) throw new DuplicateDataException();
    
    Dept saveDept = deptRepository.save(deptDto.toEntity());

    return saveDept.toDto();
  }

  ...
}
```

---

### 2. Contoller Advice 선언

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

  @ExceptionHandler(NotFoundException.class)
  public ResponseEntity<String> notFoundException(NotFoundException e) {
    return ResponseEntity.status(HttpStatus.NOT_FOUND).body(e.getMessage());
  }

  @ExceptionHandler(DuplicateDataException.class)
  public ResponseEntity<String> duplicateFoundException(DuplicateDataException e) {
    return ResponseEntity.status(HttpStatus.CONFLICT).body(e.getMessage());
  }
}
```

`@RestControllerAdvice`
- `@RestController`가 적용된 클래스에만 작동
- JSON 또는 XML과 같은 데이터 형식을 반환
- ESTful 웹 서비스에서 사용

`@ExceptionHandler`
- 지정한 예외가 발생하면 적용된 메서드를 실행
- 괄호 안에 `{예외이름}.class` 형식으로 가로챌 예외를 지정

앞서 선언한 커스텀 예외가 발생하면 `ResponseEntity` 객체를 반환
- 이 때 `status`와 `body`에 예외에 맞는 에러 코드와 메시지를 담음

---

### 3. DTO 유효성 검증

DTO의 각 필드의 유효성을 검사하여 통과하지 못하면 예외를 발생시킬 수 있다.
- RestControllerAdvice에서 해당 예외를 가로채 처리 가능

우선 application.properties에 의존성 주입부터 실행

```
dependencies {
  ...
	implementation 'org.springframework.boot:spring-boot-starter-validation'
  ...
}
```

DTO 클래스에서 유효성 검사를 원하는 필드를 지정
- 다양한 어노테이션을 활용해 유효값 지정 가능
- 이 때 검사를 통과하지 못 하면 반환할 에러메시지 지정 가능

```java
public class DeptDto {

  @NotNull(message = "부서번호는 필수 입력요소입니다.")
  private Long deptno;

  @NotBlank(message = "부서이름은 필수 입력요소입니다.")
  private String dname;

  private String loc;

  public Dept toEntity() {
    Dept dept = Dept.builder()
                    .deptno(this.deptno)
                    .dname(this.dname)
                    .loc(this.loc)
                    .build();
      
    return dept;
  }
}
```

Controller에서 DTO 객체를 인자로 받을 때 유효성 검사 실행
- `@Valid`을 통해 지정 가능

```java
@RestController
@RequestMapping("/api/dept")
@RequiredArgsConstructor
public class DeptController {
  ...

  // 생성
  @PostMapping("")
  public ResponseEntity<DeptDto> createDept(@RequestBody @Valid DeptDto deptDto) {

    DeptDto savedDeptDtp = deptService.createDept(deptDto);
    
    return ResponseEntity.status(HttpStatus.OK).body(savedDeptDtp);
  }
  
}
```

유효성 검증을 통과하지 못 하면 수행할 메서드를 `RestControllerAdvice`에 지정
- 이 때 예외 클래스는 `MethodArgumentNotValidException`이다.
- 예외 객체의 `getFieldError().getDefaultMessage()`을 통해 에러메시지에 접근 가능

```java
@RestControllerAdvice
public class GlobalExceptionHandler {
  ...

  @ExceptionHandler(MethodArgumentNotValidException.class)
  public ResponseEntity<String> methodArgumentNotValidException(MethodArgumentNotValidException e) {
    return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(e.getFieldError().getDefaultMessage());
  }
}
```