---
title: Spring - Security
date: 2024-05-27 09:00:00 +09:00
categories: [Back-End, Spring]
tags: [Back-End, Java, Spring, Security]
---

## Ⅰ. 스프링 시큐리티란?

### 1. 스프링 시큐리티란

> 자바 애플리케이션에서 인증(authentication)과 권한 부여(authorization)를 처리하는 강력하고 유연한 프레임워크

#### 인증(Authentication)
- 사용자가 누구인지 확인하는 절차
- 사용자 이름과 비밀번호를 입력하는 방식이 대표적
- 스프링 시큐리티는 다양한 인증 메커니즘을 지원(폼 로그인, 기본 인증, OAuth2, OpenID Connect 등)

#### 권한 부여(Authorization)
- 인증된 사용자가 애플리케이션 내에서 어떤 자원에 접근할 수 있는지 결정하는 절차
- 이를 위해 스프링 시큐리티는 역할 기반 접근 제어(RBAC)와 같은 다양한 접근 제어 방식을 제공

#### 세션 관리
- 사용자 세션을 관리하고, 세션 고정 공격(session fixation attack)을 방지하는 기능을 제공

---

### 2. 스프링 시큐리티의 구조

![스프링 시큐리티의 구조](https://mblogthumb-phinf.pstatic.net/MjAxODA5MjdfMTgg/MDAxNTM4MDM3ODAyMTEy.GWR41ZFQxycWhrINO4oTzG7SSP1YDVO0uNvqYQzNrvUg.BqOwvNUZxUoA3Zj54g6m4WozCtvT4SxUEmeqe2IMtuwg.PNG.ithink3366/Spring_Security_%EC%9D%B8%EC%A6%9D_rnwh.png?type=w800)

#### SecurityContext
- 현재 애플리케이션의 보안 정보를 저장하는 컨테이너
- 보통 SecurityContextHolder를 통해 접근
- Authentication 객체를 포함하고 있으며, 이 객체는 현재 인증된 사용자에 대한 정보를 담고 있음
  
#### Authentication
- 인증된 사용자에 대한 정보를 나타내는 인터페이스
- 사용자 이름, 비밀번호, 권한 등을 포함
- AuthenticationManager에 의해 생성되고 관리

#### AuthenticationManager
- 다양한 AuthenticationProvider를 사용하여 인증을 처리합니다.
  
#### AuthenticationProvider
- 특정한 인증 논리를 구현하는 컴포넌트
- 여러 AuthenticationProvider가 존재할 수 있으며, 이들은 AuthenticationManager에 의해 순차적으로 호출
예를 들어, 데이터베이스 인증, LDAP 인증, OAuth2 인증 등을 처리하는 다양한 AuthenticationProvider가 있음

#### UserDetailsService
- 사용자 정보를 로드하는 인터페이스
- 주로 사용자 이름을 기반으로 UserDetails 객체를 반환
- UserDetails 객체는 사용자 이름, 비밀번호, 권한 등을 포함

#### Filter
- 스프링 시큐리티는 여러 필터를 사용하여 보안 기능을 구현
- 이 필터들은 체인 형태로 구성되어 요청을 처리

---

### 3. 스프링 시큐리티의 흐름

#### 요청 수신
- 사용자가 애플리케이션에 접근할 때 HTTP 요청이 발생

#### Filter Chain
- 요청이 스프링 시큐리티의 필터 체인을 통과
- Authentication Filterf를 통과하며, UsernamePasswordAuthenticationToken의 인증용 토큰 객체 생성
  
#### AuthenticationManager
- 인증 필터가 사용자 자격 증명을 AuthenticationManager에 전달하여 인증을 시도
- AuthenticationManager는 적절한 AuthenticationProvider를 호출하여 인증을 처리

#### UserDetailService
- UserDetailService에 사용자 정보를 전달
- UserDetailService는 넘겨받은 사용자 정보를 통해 사용자 정보인 UserDetails 객체를 만듦
- AuthenticationProvider들은 UserDetails를 넘겨받아 정보를 비교

#### SecurityContext
- 성공적으로 인증되면, Authentication 객체가 생성되고 SecurityContext에 저장
- SecurityContextHolder를 통해 SecurityContext에 접근 가능
- 현재 인증된 사용자 정보를 포함

---
<br>

## Ⅱ. 스프링 레거시 Security - 기본 설정

### 1. pom.xml

- 이 포스트에선 Maven 프로젝트를 사용
- pom.xml에 의존성 주입

```xml
<dependencies>
  ...
  <dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-web</artifactId>
    <version>4.0.1.RELEASE</version>
  </dependency>
  <dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-config</artifactId>
    <version>4.0.1.RELEASE</version>
  </dependency>
  <dependency>
      <groupId>org.springframework.security</groupId>
      <artifactId>spring-security-taglibs</artifactId>
      <version>4.0.1.RELEASE</version>
  </dependency>
  ...
</dependencies>
```

---

### 2. web.xml

- web.xml에 root-context로 사용하겠다고 선언
- 이후 선언한 위치에 동일한 파일 생성

```xml
<context-param>
  <param-name>contextConfigLocation</param-name>
  ...
  <param-value>/WEB-INF/spring/appService/security-context.xml</param-value>
</context-param>
```

- 인증 권한 관련 필터 추가

```xml
<filter>
  <filter-name>springSecurityFilterChain</filter-name>
  <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
</filter>
<filter-mapping>
  <filter-name>springSecurityFilterChain</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>
```

---

### 3. security-context.xml

- `xmlns:security="http://www.springframework.org/schema/security"` 추가
- `xsi:schemaLocation`에  
  `http://www.springframework.org/schema/security`와  
  `http://www.springframework.org/schema/security/spring-security.xsd` 추가

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:context="http://www.springframework.org/schema/context"
  xmlns:security="http://www.springframework.org/schema/security"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://www.springframework.org/schema/beans 
                      http://www.springframework.org/schema/beans/spring-beans.xsd
                      http://www.springframework.org/schema/context 
                      http://www.springframework.org/schema/context/spring-context.xsd
                      http://www.springframework.org/schema/security http://www.springframework.org/schema/security/spring-security.xsd">
</beans>
```

---
<br>

## Ⅲ. 1단계 - In-Memory 방식

### 1. security-context.xml

```xml
<bean>
<security:http auto-config="true">
  <security:intercept-url pattern="/customer/noticeDetail.htm" access="hasRole('ROLE_USER')"/>
  <security:intercept-url pattern="/customer/noticeReg.htm" access="hasRole('ROLE_ADMIN')"/>
</security:http>

<security:authentication-manager>
  <security:authentication-provider>
    <security:user-service>
      <security:user name="kglim" password="1004" authorities="ROLE_USER"/>
      <security:user name="admin" password="1004" authorities="ROLE_ADMIN,ROLE_USER"/>
    </security:user-service>
  </security:authentication-provider>
</security:authentication-manager>

</bean>
```

`security:intercept-url`
- `/customer/noticeDetail.htm` : `ROLE_USER` 역할을 가진 사용자만 접근 가능
- `/customer/noticeReg.htm` : `ROLE_ADMIN` 역할을 가진 사용자만 접근 가능

`security:authentication-manager`
- `kglim` 사용자는 `ROLE_USER` 권한을 가지며, `admin`사용자는 `ROLE_ADMIN`과 `ROLE_USER` 권한을 가짐

`auto-config="true"`
- 스프링이 정의하는 환경 설정을 그대로 따라 가겠다고 선언
- 스프링은 로그폼도 별도로 제공
- in-memory 방식으로 프로그램에서 계정과 비번을 세팅해서 작업 처리  
- 실제로 DB(DAO) 방법으로 처리하는 것이 원칙 (DB연동 전에)

---

### 2. 인증의 흐름
- `/login` 요청 (인증처리) : Spring 내부적으로 Controller 구현되어 있고 method 구현되어 있고 자동 매핑
- 미리 설정해둔 name과 password와 비교 >> 인증 확인 >> session활용 저장 >> 자동화
- `/login?error` : 인증실패 
- `/logout` 요청하면 >> 로그 아웃처리 >> session 소멸   

---
<br>

## Ⅳ. 2단계 - 로그인, 회원가입폼을 별도로 작성

### 1. security-context.xml

```xml
<security:http auto-config="true">
  <security:csrf disabled="true" />
  <security:form-login login-page="/joinus/login.htm" 
            default-target-url="/index.htm"
            authentication-failure-url="/joinus/login.htm?error"/>
  <security:logout logout-success-url="/index.htm" />
  
  <security:intercept-url pattern="/customer/noticeDetail.htm" access="hasRole('ROLE_USER')"/>
  <security:intercept-url pattern="/customer/noticeReg.htm" access="hasRole('ROLE_ADMIN')"/>
</security:http>

<security:authentication-manager>
  <security:authentication-provider>
    <security:user-service>
      <security:user name="hong"  password="1004" authorities="ROLE_USER"/>
      <security:user name="admin" password="1004" authorities="ROLE_USER,ROLE_ADMIN"/>
    </security:user-service>
  </security:authentication-provider>
</security:authentication-manager>
```

`auto-config="true"`
- Spring Security의 기본적인 설정을 자동으로 구성

`<security:csrf disabled="true" />`
- CSRF (Cross-Site Request Forgery) 보호를 비활성화
- 일반적으로 보안상의 이유로 CSRF 보호를 활성화하는 것이 좋음
  
`<security:form-login>`
- 로그인 폼 설정을 정의
- `login-page="/joinus/login.htm"`: 사용자가 인증되지 않은 상태 접근하면 이 로그인 페이지로 리다이렉트
- `default-target-url="/index.htm"`: 로그인 성공 후 사용자를 리다이렉트할 URL
- `authentication-failure-url="/joinus/login.htm?error"`: 인증 실패 시 리다이렉트될 URL
  
`<security:logout>`
- 로그아웃 설정을 정의합니다.
- `logout-success-url="/index.htm"`: 로그아웃 성공 후 사용자를 리다이렉트할 URL
  
`<security:intercept-url>`
- URL 접근 제어를 정의합니다.
- `pattern="/customer/noticeDetail.htm"`: 이 URL에 접근하려면 ROLE_USER 권한이 필요
- `pattern="/customer/noticeReg.htm"`: 이 URL에 접근하려면 ROLE_ADMIN 권한이 필요

---

### 2. Controller

```java
@Controller
@RequestMapping("/joinus/")
public class JoinController {

	// memberdao 의존
	private MemberService memberservice;

	@Autowired
	public void setMemberservice(MemberService memberservice) {
		this.memberservice = memberservice;
	}
	
	//GET 요청
	//join.jsp 화면
	@GetMapping("join.htm")  //  /joinus/join.htm
	public String join() {
		return "joinus/join";
	}

	//POST 요청
	@PostMapping("join.htm")
	public String join(Member member) {
		 String url= null;
		 try {
			   url = memberservice.insert(member);
		} catch (Exception e) {
			e.printStackTrace();
		}
		return url;

	}
	
	//로그인 요청 
	@GetMapping("login.htm")
	public String login() {
		return "joinus/login";
	}
	
}
```

- 로그인 요청시 전달할 뷰페이지를 정의

---

### 3. login.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>	
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en" lang="en">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>index</title>
<link href="login.css" type="text/css" rel="stylesheet" />
</head>
<body>
  <jsp:include page="/WEB-INF/views/inc/header.jsp" />
  <jsp:include page="inc/visual.jsp" />
  <div id="main">
    <div class="top-wrapper clear">
      <div id="content">
        <h2>로그인</h2>
        <h3 class="hidden">방문페이지 로그</h3>
        <ul id="breadscrumb" class="block_hlist clear">
          <li>HOME</li>
          <li>회원가입</li>
          <li>로그인</li>
        </ul>
        <h3 class="hidden">회원가입 폼</h3>
        <div id="join-form" class="join-form margin-large">
          <c:if test="${param.error != null }">
            <div>
              로그인 실패<br>
                  <c:if test="${SPRING_SECURITY_LAST_EXCEPTION != null}">
                    이유 : <c:out value="${SPRING_SECURITY_LAST_EXCEPTION.message}" />
                  </c:if>
            </div>
          </c:if>				
  
          <form action="${pageContext.request.contextPath}/login" method="post">
            <fieldset>
              <legend class="hidden">로그인 폼</legend>
              <h3>
                <img src="images/txtTitle.png" />
              </h3>
              <ul id="loginBox">
                <li><label for="uid">아이디</label>
                <input name="username"	class="text" /></li>
                <li><label for="pwd">비밀번호</label>
                <input type="password"	name="password" class="text" /></li>
              </ul>
              <p>
                <input type="submit" id="btnLogin" value="" />
              </p>
              <ul id="loginOption">
                <li><span>아이디 또는 비밀번호를 분실하셨나요?</span><a
                  href="/Member/FindID"><img alt="ID/PWD 찾기"
                    src="images/btnFind.png" /></a></li>
                <li><span>아이디가 없으신 분은 회원가입을 해주세요.</span><a
                  href="/Member/Agree"><img alt="회원가입"
                    src="images/btnJoin.png" /></a></li>
              </ul>
            </fieldset>
          </form>
        </div>

      </div>
      <jsp:include page="inc/aside.jsp" />
    </div>
  </div>
  <jsp:include page="/WEB-INF/views/inc/footer.jsp" />
</body>
</html>
```

- jstl if문을 사용하여 로그인 실패 여부 확인
- 로그인 실패시 에러 메시지를 시큐리티로부터 받아옴
- 아이디와 비밀번호 정보를 시큐리티 프레임워크에 전달

---

### 4. header.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"%>

<%@ taglib prefix="c"         uri="http://java.sun.com/jsp/jstl/core" %>    
<%@ taglib prefix="security"  uri="http://www.springframework.org/security/tags" %>

  
<div id="header">
  <div class="top-wrapper">
    <h1 id="logo">
      <a href="/"><img src="" alt="로고" /></a>
    </h1>
    <h2 class="hidden">메인메뉴</h2>
    <ul id="mainmenu" class="block_hlist">
      <li><a href="">kosa가이드</a></li>
      <li><a href="">kosa과정</a></li>
      <li><a href="">kosa</a></li>
    </ul>
    <form id="searchform" action="" method="get">
      <fieldset>
        <legend class="hidden"> 과정검색폼 </legend>
        <label for="query">과정검색</label> <input type="text" name="query" />
        <input type="submit" class="button" value="검색" />
      </fieldset>
    </form>
    <h3 class="hidden">로그인메뉴</h3>
    <ul id="loginmenu" class="block_hlist">
      <li><a href="${pageContext.request.contextPath}/index.htm">HOME</a></li>
      
      <security:authorize access="!hasRole('ROLE_USER')"><!-- 인증되지 않았다면, 로그인 하지 않았다면  -->
        <li><a href="${pageContext.request.contextPath}/joinus/login.htm">로그인</a></li>
      </security:authorize>
      
      <!-- 인증이 성공되면 Spring 내부적으로 userPrincipal 객체의 name 속성에 인증값(id >> name)  -->
      <security:authentication property="name" var="loginuser" />
      <!-- loginuser 변수에 인증된 name(id) 있어요  -->
      
      <!-- Any -> OR   ROLE_USER 또는 ROLE_ADMIN 라면 -->
      <security:authorize access="hasAnyRole('ROLE_USER','ROLE_ADMIN')"> 
          <li><a href="${pageContext.request.contextPath}/logout">${loginuser}:로그아웃</a></li>
      </security:authorize>
      
      <li><a href="${pageContext.request.contextPath}/joinus/join.htm">회원가입</a></li>
    </ul>
    <h3 class="hidden">회원메뉴</h3>
    <ul id="membermenu" class="clear">
      <li><a href=""><img src="${pageContext.request.contextPath}/images/menuMyPage.png" alt="마이페이지" /></a>
      </li>
      <li><a href="${pageContext.request.contextPath}/customer/notice.htm">
          <img src="${pageContext.request.contextPath}/images/menuCustomer.png" alt="고객센터" /></a></li>
    </ul>
  </div>
</div>
```

`<%@ taglib prefix="security"  uri="http://www.springframework.org/security/tags" %>`
- JSP 페이지에서 Spring Security의 태그 라이브러리를 선언하는 부분
- JSP 페이지에서 보안 관련 기능을 쉽게 사용할 수 있음

`<security:authorize>`
- 권한에 따라 다른 메뉴를 보여줍니다.
- 인증된 사용자의 이름을 `${loginuser}`로 표시합니다.

---
<br>

## Ⅴ. 인증된 사용자 정보 활용하기

### 1. SecurityContextHolder

```java
public String noticeReg(Notice n, HttpServletRequest request) {
  ...
  
  // 인증된 사용자 정보를 가지고 오기
  SecurityContext context = SecurityContextHolder.getContext();
  Authentication auth = context.getAuthentication();
  UserDetails userinfo = (UserDetails)auth.getPrincipal();
  
  n.setWriter(userinfo.getUsername());
  
  ...

  try {
    NoticeDao noticedao = sqlsession.getMapper(NoticeDao.class);
    noticedao.insert(n); // DB insert
  } catch (Exception e) {
    e.printStackTrace();
  }

  return "redirect:notice.htm";
}
```

`SecurityContextHolder.getContext()`
- 현재 사용자의 보안 컨텍스트를 가져옴

`getAuthentication()`
- 현재 사용자의 인증 정보를 가져옴
- 이 정보는 UserDetails 인터페이스를 구현한 객체

`userinfo.getUsername()은 `
- 현재 사용자의 이름을 반환

--- 

### 2. Principal

```java
public String noticeReg(Notice n, HttpServletRequest request, Principal principal) {
  ...

  // 인증된 사용자 정보를 가지고 오기
  n.setWriter(principal.getName());
  
  ...

  try {
    NoticeDao noticedao = sqlsession.getMapper(NoticeDao.class);
    noticedao.insert(n); // DB insert
  } catch (Exception e) {
    e.printStackTrace();
  }

  return "redirect:notice.htm";
}
```

- Principal 객체를 통해 현재 사용자의 인증 정보를 가져온다.
- `getName()` 메서드를 통해 사용자의 이름을 반환

---
<br>

## Ⅵ. DB 작업

### 1. 필요한 테이블 및 컬럼

- USER : 유저 정보
- ROLL : 권한 정보
- USERANDROLL(다대다) : 한 유저가 다수의 권한을 보유할 수 있기 때문

여기서 USERANDROLL 테이블은 상황에 따라 생략 가능

---

### 2. security-context.xml

- JDBC를 사용하여 DB와 연동처리 진행

```xml
<security:http auto-config="true">
  <security:csrf disabled="true" />
  <security:form-login
    login-page="/joinus/login.htm" default-target-url="/index.htm"
    authentication-failure-url="/joinus/login.htm?error" />
  <security:logout logout-success-url="/index.htm" />
  <!-- 정규 표현식을 통해 포괄적으로 url 인터셉터를 정의 -->
  <security:intercept-url pattern="/customer/*Reg.htm" access="hasRole('ROLE_USER')" />
  <security:intercept-url pattern="/admin/**" access="hasRole('ROLE_ADMIN')" />
</security:http>

<security:authentication-manager>
  <security:authentication-provider>
    <security:jdbc-user-service data-source-ref="driverManagerDataSource"
    users-by-username-query="SELECT USERID, pwd AS PASSWORD, 1 enabled FROM member where userid=?"
    authorities-by-username-query="select m.USERID , r.ROLE_NAME from member m join roll r on m.userid = r.userid where m.userid=?"/>
  </security:authentication-provider>
</security:authentication-manager>
```

`<security:jdbc-user-service>`
- 데이터베이스에서 사용자 정보를 조회하기 위한 서비스 설정
- `data-source-ref`: 데이터 소스 빈의 참조를 지정
- `users-by-username-query`: 사용자 정보를 조회하기 위한 SQL 쿼리를 정의
- `authorities-by-username-query`: 사용자의 권한 정보를 조회하기 위한 SQL 쿼리를 정의

---
<br>

## Ⅶ. 비밀번호 암호화

### 1. bCryptPasswordEncoder

> Spring Security 프레임워크에서 제공하는 비밀번호 암호화 유틸리티

- bcrypt 해시 함수를 사용하여 비밀번호를 암호화
- 저장된 해시와 사용자가 입력한 비밀번호를 비교하는 데 사용

---

### 2. 설정

다음과 같이 Root Context와 Security Context에 의존성을 주입해준다.

```xml
<!-- Root  Context  -->
<bean id="bCryptPasswordEncoder"   class="org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder">
</bean>

<!-- Security Context  -->
<security:password-encoder ref="bCryptPasswordEncoder"/>
```

---

### 3. 데이터베이스 칼럼 타입 변경

암호화된 데이터는 기존 varchar2 타입이 감당하지 못 할 가능성이 높다.
- 최대 수용량을 늘려줘야한다.

```sql
alter table member
modify (pwd varchar2(2000));
```

---

### 4. Controller 설정

```java
@Controller
@RequestMapping("/join/")
public class JoinController {

	@Autowired
	private View jsonview;

	@Autowired
	private JoinService service;

	@Autowired
	private BCryptPasswordEncoder bCryptPasswordEncoder;

  ...

  // 회원가입
	@RequestMapping(value = "join.htm", method = RequestMethod.POST) 
	public String join(Member member) {
		int result = 0;
		String viewpage = "";
		member.setPwd(this.bCryptPasswordEncoder.encode(member.getPwd())); //암호화 
		result = service.insertMember(member);
		if (result > 0) {
			System.out.println("삽입 성공");
			viewpage = "redirect:/index.htm";
		} else {
			System.out.println("삽입 실패");
			viewpage = "join.htm";
		}
		return viewpage;
	}

  ...
}
```

- `bCryptPasswordEncoder`의 의존성 주입
- `bCryptPasswordEncoder` 객체의 `encode` 메서드를 통해 암호화한 비밀번호를 입력

```java
@Controller
@RequestMapping("/join/")
public class MemberController {

	@Autowired
	private MemberService service;
	
	@Autowired
	private BCryptPasswordEncoder bCryptPasswordEncoder;
	
  // 회원 확인
	@RequestMapping(value="memberconfirm.htm",method=RequestMethod.POST)
	public String memberConfirm(@RequestParam("password") String rawPassword,	Principal principal){
		String viewpage="";
		
		//회원정보
		Member member = service.getMember(principal.getName());
		
		//DB에서 가져온 암호화된 문자열
		String encodedPassword = member.getPwd();
		
    // 비밀번호 확인
		boolean result = bCryptPasswordEncoder.matches(rawPassword, encodedPassword);
		
		if(result){
			viewpage="redirect:memberupdate.htm";
		}else{
			viewpage="redirect:memberconfirm.htm";
		}
		
		return viewpage;
	}
	
	// 회원 정보 업데이트
	@RequestMapping(value="memberupdate.htm", method=RequestMethod.POST)
	public String memberUpdate(Model model, Member member, Principal principal){
		
		Member updatemember = service.getMember(principal.getName());
		
		updatemember.setName(member.getName());
		updatemember.setCphone(member.getCphone());
		updatemember.setEmail(member.getEmail());
		updatemember.setPwd(bCryptPasswordEncoder.encode(member.getPwd()));
		service.updateMember(updatemember);
		return "redirect:/index.htm";
	}
}
```

- 비밀번호 일치 여부 확인은 `bCryptPasswordEncoder`객체의 `matches` 메서드 확인
- 회원정보 업데이트 시 비밀번호 인코딩 또한 회원가입과 동일하게 진행