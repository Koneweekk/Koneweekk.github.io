---
title: Spring Boot - Security
date: 2024-05-28 14:00:00 +09:00
categories: [Back-End, Spring Boot]
tags: [Back-End, Java, Spring Boot, Security]
---

## Ⅰ. 기본 설정

### 1. build.gradle

사용하고 있는 스프링부트의 버전에 따라 적절한 security 프레임워크의 버전 선택

```
dependencies {
  ...
	implementation 'org.springframework.security:spring-security-core:5.5.2' // 원하는 버전으로 변경
	implementation 'org.springframework.security:spring-security-web:5.5.2'  // 원하는 버전으로 변경
	implementation 'org.springframework.security:spring-security-config:5.5.2'  // 원하는 버전으로 변경
  ...
}
```

사용할 DB에 맞게 의존성 추가

```
dependencies {
  ...
	implementation 'org.springframework.boot:spring-boot-starter-jdbc'
	runtimeOnly 'com.mysql:mysql-connector-j'
  ...
}
```

---

### 2. DB 연결

application.properties에 다음과 같이 추가
- 이 글에선 MySQL을 사용

```
server.port=[포트번호]

spring.datasource.url=jdbc:mysql://localhost:[MySQL포트번호]/[DB이름]
spring.datasource.username=[사용자이름]
spring.datasource.password=[설정비밃번호]
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQLDialect
```

---
<br>

## Ⅱ. Security Configuration

### 1. Security Configuration란?

> 애플리케이션의 보안 설정을 구성하는 작업을 의미

- Spring Security는 강력하고 유연한 보안 프레임워크로, 인증(authentication)과 권한 부여(authorization) 기능을 제공
- Security Configuration을 통해 애플리케이션에 접근하는 사용자와 그들의 권한을 관리

---

### 2. Config 파일 생성

config란 패키지를 생성하여 SecurityConfig 클래스 파일 생성

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
  ...
}
```

`@Configuration`
- 클래스가 하나이상의 `@Bean` 메서드를 포함하고 있다고 선언
- Spring IoC 컨테이너가 이 클래스의 인스턴스를 생성 및 관리

`@EnableWebSecurity`
- Spring Security 설정을 활성화
- `WebSecurityConfigurerAdapter` 확장한 구성 클래스에서 보안 설정을 정의

---

### 3. FilterChain

`SecurityFilterChain`를 정의하는 빈을 선언
- Spring Security의 필터 체인을 구성하여 HTTP 요청에 대한 보안 규칙을 설정

```java
@Bean
public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {

    http.authorizeRequests()
            .antMatchers("/admin", "/admin/**").hasRole("ADMIN")
            .antMatchers("/user", "/user/**").hasAnyRole("USER", "ADMIN")
            .antMatchers("/css/**", "/js/**", "/imges/**").permitAll()
            .antMatchers("/**").permitAll()
            //설정하지 않은 모든 경로 요청에 대해서 인증된 사용자만 접근 가능
            .anyRequest().authenticated();
    http.formLogin();
    return http.build();
}
```

`authorizeRequests()`
- antMatchersURL 경로에 따라 접근 권한을 설정합니다.
- `antMatchers` : 접근 권한을 설정할 URL을 지정
- `hasRole`, `hasRole`, `permitAll` : 해당 URL에 접근을 허용할 권한을 지정(단수, 다수, 전부)
- `anyRequest().authenticated()` : 위에서 설정한 경로 외의 모든 요청은 인증된 사용자만 접근할 수 있도록 설정

`http.build()`
- 구성된 `HttpSecurity` 객체를 빌드하여 반환합니다.

---

### 4. authenticationManager

Spring Security에서 인증을 처리하는 주요 컴포넌트

```java
@Bean
public AuthenticationManager authenticationManager(AuthenticationConfiguration authenticationConfiguration) throws Exception {
    return authenticationConfiguration.getAuthenticationManager();
}
```

- `AuthenticationConfiguration`을 통해 `AuthenticationManager`를 가져와 반환합니다.

---

### 5. passwordEncoder 

비밀번호를 암호화하는 역할

```java
@Bean
public PasswordEncoder passwordEncoder() {
    return new BCryptPasswordEncoder();
}
```

- BCrypt 알고리즘을 사용하여 비밀번호를 암호화
- 강력한 해시함수의 일종

---

### 6. In-Memory 인증

코드에서 미리 정해둔 데이터로 사용자 인증
- 아래 코드에선 일반 유저(`user`)와 관리자(`admin`) 한명씩 정의

```java
@Bean
public UserDetailsService userDetailsService() {
  
  UserDetails user = User.builder()
                .username("user")
                .password(passwordEncoder().encode("1004")) //boot 기본적으로 암호화된 비번을 사용 
                .roles("USER")
                .build();
  
  UserDetails admin = User.builder()
            .username("admin")
            .password(passwordEncoder().encode("1004"))
            .roles("USER","ADMIN")
            .build();

  return new InMemoryUserDetailsManager(user,admin);
  
}
```

`UserDetails` 
- 사용자의 세부 정보를 담는 객체입니다.
- `User.builder()`를 사용하여 사용자를 쉽게 생성
- `username` : 사용자 이름을 설정
- `password` : 암호화된 비밀번호를 설정
- `roles` : 사용자의 역할을 설정

`InMemoryUserDetailsManager`
- `UserDetailsService` 인터페이스의 구현체로, 메모리 내에서 사용자 정보를 관리
- 생성자에 `UserDetails` 객체를 전달하여 초기 사용자 정보를 설정

---

### 7. DB를 통한 검증

JDBC를 사용해 DB에 저장된 사용자 데이터를 이용해 사용자 인증을 수행

```java
// 데이터베이스 연결 소스
@Autowired
private DataSource datasource;

@Bean
public UserDetailsService userDetailsService() {
  
  JdbcUserDetailsManager userDetailsManager = new JdbcUserDetailsManager(datasource);
  
  //사용자 인증 쿼리
  String sql1 = "select user_id as username , user_pw as password , enabled from user where user_id=?";
  
  //사용자 권한 쿼리
  String sql2 = "select user_id as username , auth from user_auth where user_id=?";
  
  userDetailsManager.setUsersByUsernameQuery(sql1); //계정 비번 확인
  userDetailsManager.setAuthoritiesByUsernameQuery(sql2);
  
  return userDetailsManager;
}
```

`JdbcUserDetailsManager` 
- JDBC를 통해 사용자 정보를 로드하는 클래스
- 생성자에 DataSource를 주입하여 데이터베이스 연결을 설정

`userDetailsManager.setUsersByUsernameQuery`
- 사용자 정보를 조회하는 SQL 쿼리를 설정

`userDetailsManager.setAuthoritiesByUsernameQuery`
- 사용자 권한을 조회하는 SQL 쿼리를 설정