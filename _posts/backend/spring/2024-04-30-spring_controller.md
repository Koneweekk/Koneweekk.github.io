---
title: Spring - 컨트롤러
date: 2024-04-30 10:30:00 +09:00
categories: [Back-End, Spring]
tags: [Back-End, Java, Spring, Annotation]
---

## Ⅰ. @Controller

### 1. @Controller란?

> 컨트롤러 역할을 하는 클래스를 표시하는 어노테이션

- Presentation Layer에서 Contoller를 명시하기 위해서 사용
- Controller 안에서 함수 단위 매핑 가능

---

### 2. @RequestMapping()

- HTTP 요청에 대한 처리를 담당하는 메소드
- 이러한 메소드들은 클라이언트의 요청을 받아 비즈니스 로직을 수행
- 모델을 생성하거나 뷰를 렌더링하여 클라이언트에게 응답을 반환

```java
import java.util.Calendar;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

@Controller
public class HelloController {
	public HelloController() {
		System.out.println("HelloController 생성자 호출");
	}
	
	@RequestMapping("/hello.do")
	public ModelAndView hello() {
		System.out.println("hello.do method call");
		ModelAndView mav = new ModelAndView();
		mav.addObject("greeting",getGreeting());
		mav.setViewName("Hello");
		return mav;
	}

	private String getGreeting() {
		int hour = Calendar.getInstance().get(Calendar.HOUR_OF_DAY);
		String data="";
		if(hour >= 6 && hour <= 10) {
			data="학습시간";
		}else if(hour >= 11 && hour <= 13) {
			data="배고픈시간";
		}else if(hour >= 14 && hour <= 18) {
			data="졸려운시간";
		}else {
			data="go home";
		}
		return data;
		
	}
}
```

- `hello()` 메서드를 `@RequestMapping("/hello.do") `어노테이션으로 지정
- `"/hello.do"` 경로로 들어오는 HTTP 요청을 처리

---

### 3. GET, POST

`@GetMapping`
- 특정 URL 경로로의 HTTP GET 요청을 처리하는 메서드를 정의할 때 사용
- 반환한 문자열에 해당하는 view를 매핑하여 이동시킴

```java
@GetMapping
public String form() {  //화면
  return "article/newArticleForm";
}
```

`@PostMapping`
- 특정 URL 경로로의 HTTP POST 요청을 처리하는 메서드를 정의할 때 사용
- 반환한 문자열에 해당하는 view를 매핑하여 이동시킴

```java
@PostMapping  //5.x.x
public ModelAndView sumbit(NewArticleCommand command) { //처리 

  this.articleService.writeArticle(command);
  ModelAndView mv = new ModelAndView();
  mv.addObject("newArticleCommand", command);  
  mv.setViewName("article/newArticleSubmitted");
  
  return mv;
}
```

※ input 태그의 name값이 DTO 객체의 memberfield 명과 동일하면 자동으로 DTO객체를 매개변수로 사용 가능

1. 자동 DTO 객체 생성
2. 넘어온 parameter setter 함수를 통해서 자동 주입
3. DTO 객체 자동 IOC 컨테이너 안에 등록

---

### 4. @ModelAttribute

- HTTP 요청의 데이터를 메서드 매개변수에 바인딩할 때 사용
- 컨트롤러 메서드에 전달되는 데이터를 특정 모델 객체에 바인딩

```java
@PostMapping
public String sumbit(@ModelAttribute("Articledata") NewArticleCommand command) {

  this.articleService.writeArticle(command);
  
  return "article/newArticleSubmitted";
}
```

`@ModelAttribute("Articledata")`
- `forward`를 통해 이동한 view 페이지에서 `Articledata`를 객체명으로 사용 가능

```html
<body>
	<h3>게시판 입력 내용 확인</h3>

	제목  ${Articledata.title}<br>
	내용  ${Articledata.content}<br>
	순번  ${Articledata.parentId}<br> 
</body>
```

---

### 5. Model

`ModelAndView` 대신 사용할 수 있는 매개변수 타입

```java
@RequestMapping("noticeDetail.htm")
public String noticeDetail(@RequestParam("seq") String seq, Model model) throws Exception {

  ModelAndView mav = new ModelAndView();
  
  Notice notice = noticsdao.getNotice(seq);
  model.addAttribute("notice", notice);
  
  return "noticeDetail";
}
```

`Model` 객체에 데이터를 담고, `noticeDetail`라는 view 파일로 경로 지정
- 별도의 `setView` 메서드 필요 X

### ※ context:component-scan

`<context:component-scan base-package="com.example.package" />`
- 위 구문을 servlet xml 파일에 추가
- 입력한 패키지에 아래 어노테이션을 지닌 클래스가 있다면 모두 자동 빈 객체 생성
- `@Controller`, `@Service`, `@Repository`, `@Component`

---
<br>

## Ⅱ. 매개 변수 입력받기

### 1. request 객체 이용

- 스프링이 아니더라도 통용되는 방법
- `getParameter` 메서드 활용

```java
public ModelAndView searchExternal(HttpServletRequest request) {
  String id= request.getParameter("id")
} 
```

---

### 2. DTO 객체를 통한 전달 방법

- input 태그의 name과 dto의 맴버필드가 모두 같으면 객체에 자동 매핑
- 자동으로 request 객체에 dto 객체를 추가하여 forward

```java
	  public ModelAndView searchExternal(MemberDto member){
	      private String id;
	      private String name;
	       
	  }
```

---

### 3. 매개변수 이름 입력

- 이때 url에 입력되는 파라미터 이름과 변수명이 정확히 일치해야한다.

```java
  public ModelAndView searchExternal(String id, String name , int age){
    ...
  }
```

---

### 4. @RequestParam

- 파라미터 이름을 지정 가능
- null값이라면 초기값 설정 가능
- 변수면 지정 가능

```java
public ModelAndView searchExternal(
  @RequestParam(value="query" , defaultValue = "kosa") String query,
  @RequestParam(value="p", defaultValue = "10") int p) {

  ...
}
```

---

### 5. REST 방식 (비동기 처리) 

- URL의 일부를 동적으로 처리해야 할 때 사용

```java
@GetMapping("/customer/{id}")
public String getCustomerById(@PathVariable("id") Long customerId, Model model) {
  ...
}
```

- 여기서 {id} 부분은 경로 변수
- 이 경로 변수의 값은 실제 요청이 들어올 때 동적으로 지정

---
<br>

## Ⅲ. 쿠키 컨트롤

### 1. 쿠키 추가하기

- servlet과 동일
- `response`객체와 `addCookie` 메서드 활용

```java
@RequestMapping("/cookie/make.do")
public String make(HttpServletResponse response) {
  
  response.addCookie(new Cookie("auth", "1004")); //servlet 동일
  
  ...
}
```

---

### 2. 쿠키 읽어오기

- `@CookieValue` 어노테이션 활용
- `value` : 읽어올 쿠키 지정
- `defaultValue` : 읽어올 쿠키가 없을 경우의 초기값
- 쿠키 값을 담을 변수명, 변수타입 지정 가능

```java
@RequestMapping("/cookie/view.do")
public String view(@CookieValue(value="auth" , defaultValue = "1007") String auth) {
  ...
}
```

---
<br>

## Ⅳ. 파일 업로드

### 1. CommonsMultipartResolver

- 업로드한 파일에 대한 정보 관리(크기 , 이름  , 중복이름 정책 등등)

```xml
<bean id="multipartResolver"  class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
  <property name="maxUploadSize" value="1048760" />
  <property name="defaultEncoding" value="UTF-8" />
</bean>
```

---

### 2. CommonsMultipartFile

- 업로드할 파일을 처리
- 사용하기 위해선 엄격한 조건을 지님

```html
<input type="file" name="file">
<form method="post" enctype="multipart/form-data">
```

- 필드명과 `input` 타입의 `name`이 동일해야함
- 인코딩타입(`enctype`)은 반드시 `multipart/form-data`

```java
import org.springframework.web.multipart.commons.CommonsMultipartFile;

public class Photo {
  ...
	private CommonsMultipartFile file; /
}
```

---

### 3. 파일 저장

- `FileOutputStream` 활용
- 파일을 저장할 서버내 경로를 지정

```java
String filename = imagefile.getOriginalFilename();
String path = request.getServletContext().getRealPath("/upload");  // 배포된 서버의 경로
String fpath = path + "\\" + filename;  

FileOutputStream fs = null;

try {
    fs = new FileOutputStream(fpath);  // 없으면 파일 생성 
    fs.write(imagefile.getBytes());
} catch (Exception e) {
      e.printStackTrace();
}finally {
  try {
      fs.close();
  } catch (Exception e2) {
    e2.printStackTrace();
  }
}
```

