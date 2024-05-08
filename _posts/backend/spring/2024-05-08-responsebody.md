---
title: Spring - ResponseBody
date: 2024-05-08 09:00:00 +09:00
categories: [Back-End, Spring]
tags: [Back-End, Java, Spring]
---

## @ResponseBody

### 1. @ResponseBody란?

> 해당 메서드가 HTTP 응답의 본문(body)으로 데이터를 직접 반환할 것임을 나타냄

- 이를 통해 메서드에서 생성한 객체나 데이터를 직렬화하여 HTTP 응답의 본문에 포함시킬 수 있음
- 해당 객체가 자동으로 JSON 타입으로 변환되어 반환
- js 단에서 따로 parsing 해줄 필요가 없음

---

### 2. 의존성 추가

Maven의 경우 아래와 pom.xml에 아래와 같은 의존성 주입이 필요하다.

```xml
<dependency>
    <groupId>org.codehaus.jackson</groupId>
    <artifactId>jackson-core-asl</artifactId>
    <version>1.9.13</version>
</dependency>
```

---

### 3. 사용 예시

```java
@RequestMapping("response.example")
public @ResponseBody Employee add(HttpServletRequest request, HttpServletResponse response) {

  Employee employee = new Employee();
  employee.setFirstname(request.getParameter("firstName"));
  employee.setLastname(request.getParameter("lastName"));
  employee.setEmail(request.getParameter("email"));
  System.out.println(employee.toString());
  return employee;
}
```

- `@Controller`를 통해 `Employee` 타입의 객체를 json 타입으로 변환하여 반환

```java
	//POINT
	@RequestMapping("response.example")
	public @ResponseBody Map<String, Object>  add(){

		Map<String, Object> map = new HashMap<String, Object>();
		map.put("result", "data");
		map.put("hello", "world");
		return map;
	}
```

- JAVA API 객체도 반환 가능하다.

```java
@RequestMapping("response2.kosa")
public @ResponseBody Employee add(@RequestBody Employee emp) {
  System.out.println("response2.kosa");
  System.out.println(emp.toString());
  return emp;
}
```

- `@RequestBody`로 클라이언트에서 전송된 데이터를 객체로 받는 것도 가능
- 이 때 클라이언트에선 문자열로 데이터를 변환해 전송해야한다.

---

<br>

## Ⅱ. @RestController

### 1. @RestController란?

- `@Controller` 어노테이션과 달리 각 메서드의 반환 값이 HTTP 응답 본문으로 직접 변환되어 클라이언트로 전송
- 각 메서드의 반환 값은 자동으로 HTTP 응답 본문에 직렬화되어 클라이언트로 전송
- 메서드 마다 일일이 `@ResponseBody` 달아줄 필요가 없음

---

### 2. 사용 예시

```java
@RestController
public class AjaxController {


	@RequestMapping("/emp")
	public List<Emp> ajaxResponseBody(){

		List<Emp> list = new ArrayList<Emp>();
		Emp e = new Emp();
		e.setEmpno(9999);
		e.setEname("홍길동");
		e.setJob("IT");
		e.setSal(1000);


		Emp e2 = new Emp();
		e2.setEmpno(1111);
		e2.setEname("김유신");
		e2.setJob("IT");
		e2.setSal(1000);


		list.add(e);
		list.add(e2);

		return list;
	}

	@RequestMapping("view.ajax")
	public String ViewPage() {
		System.out.println("view.ajax");
		return "view.ajax 문자열이 반환";
	}
}
```

- `@RestController`를 클래스에 달아줌으로써 메서드에 `@ReponseBody`를 생략

---
<br>

## Ⅲ. ResponseEntity

### 1. ResponseEntity란?

`RestController`는 별도의 View를 제공하지 않는 형태로 서비스를 실행

- 때로는 결과데이터가 예외적인 상황에서 문제가 발생

`ResponseEntity`는 개발자가 직접 결과 데이터와 HTTP 상태 코드를 직접 제어할 수 있는 클래스

- 개발자는 404나 500같은 HTTP 상태 코드를 전송하고 싶은 데이터와 함께 전송 가능
- 좀더 세밀한 제어가 필요한 경우 사용

---

### 2. 사용 예시

```java

@RequestMapping("/sendMap2")
public ResponseEntity<Map<Integer, BoardVO>> sendMap2(){

  Map<Integer, BoardVO> map = new HashMap<Integer, BoardVO>();

  for(int i=1; i <=10; i++){
    BoardVO vo = new BoardVO();
    vo.setBno(i);
    vo.setWriter("DoubleS"+i);
    vo.setContent("게시글 내용입니다"+i);
    vo.setRecnt(i);
    vo.setTitle("게시글"+i);
    vo.setUserName("DoubleS"+i);
    map.put(i, vo);
  }

  return new ResponseEntity<>(map, HttpStatus.INTERNAL_SERVER_ERROR);
}

@RequestMapping("/sendErrorAuth")
public ResponseEntity<Void> sendListAuth(){
  return new ResponseEntity<>(HttpStatus.BAD_REQUEST);
}
```

- `ResponseEntity`를 통해서 `map` 객체와 함께 상태메시지도 함께 전송
