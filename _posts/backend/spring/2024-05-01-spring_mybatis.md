---
title: Spring - MyBatis
date: 2024-05-01 09:40:00 +09:00
categories: [Back-End, Spring]
tags: [Back-End, Java, Spring, MyBatis]
---

## Ⅰ. 마이바티스 개요

### 1. 마이바티스란?

> 개발자가 지정한 SQL, 저장프로시저 그리고 몇가지 고급 매핑을 지원하는 퍼시스턴스 프레임워크

- JDBC코드와 수동으로 셋팅하는 파라미터와 결과 매핑을 제거
- 데이터베이스 레코드에 원시타입과 Map 인터페이스 그리고 자바 POJO를 설정하고 매핑하기 위해 XML과 애노테이션을 사용 가능

---

### 2. MapConfig

MyBatis 프레임워크에서 데이터베이스와의 연결 및 매핑을 설정하는 파일

드라이버 정보, 연결 URL, 사용자 이름, 암호 등의 데이터베이스 연결 정보를 설정할 수 있다.
- 설정에 필요한 정보를 가진 파일 로드 가능
- `<properties resource="SqlMapConfigExample.properties"></properties>`

SQL구문과 객체를 매핑하는 매퍼 파일 지정
- `<mapper resource="sqlMapper/User.xml" />`

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<properties resource="SqlMapConfigExample.properties"></properties>
	<environments default="development">
		<environment id="development">
			<transactionManager type="JDBC" />
			<dataSource type="POOLED">
				<property name="driver" value="${driver}" />
				<property name="url" value="${url}" />
				<property name="username" value="${username}" />
				<property name="password" value="${password}" />
			</dataSource>
		</environment>
	</environments>
	<mappers>
		<mapper resource="sqlMapper/User.xml" />
	</mappers>
</configuration>
```

---

### 3. DB연결 설정

```
driver=oracle.jdbc.driver.OracleDriver
url=jdbc:oracle:thin:@localhost:1521:XE
username=kosauser
password=1004
```

- driver : 사용하는 DB 종류
- url : DB 주소
- username : 사용자명
- password : 사용자 패스워드

---
<br>

## Ⅱ. 기본 활용법

### 1. Mapper 설정

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="Emp">
	<select id="getone" parameterType="String" resultType="kr.or.bit.dto.User">
		select empno, ename from emp where ename=#{ename}
	</select>
</mapper>
```

- 세미콜론을 넣지 않도록 주의
- `id` : 자바코드에서 호출되는 식별자를 지정 
- `#{ename}` : sql구문에 넣을 파라미터
- `parameterType` : 파라미터의 타입(1개인 경우 생략 가능 ← 자동 형변환)
- `resultType` : sql 구문을 어떤 객체와 매핑할지 지정

```xml
<select id="getUsers" resultType="kr.or.bit.dto.User">
  select empno , ename from emp
</select>
```

- 다수 행을 반환하더라도 `List<ClassType>`으로 지정 X

```xml
<select id="getUsers" resultType="kr.or.bit.dto.UserJoinDept">
  select empno , ename, deptno, dname from emp e join dept d on e.deptno=d.deptno
</select>
```

- 이런 경우처럼 join문을 활용할 경우 별도의 dto 클래스를 만드는 것이 좋다.

---

### 2. MapClient 생성

`SqlSessionFactoryBuilder`
- SqlSessionFactory 를 생성한 후 유지할 필요 x
- 메소드 스코프에서 사용하는 것이 이상적

`SqlSessionFactory`
- 한번 만든 뒤 애플리케이션을 실행하는 동안 존재해야함
- 싱글턴 패턴이나 static 싱글턴 패턴을 사용하는 것이 간단

`SqlSession`
- 요청 또는 메소드 스코프에서 사용하는 것이 좋음
- SqlSession 을 static 필드나 클래스의 인스턴스 필드로 지정해서는 안 됨
- 그리고 서블릿 프레임워크의 HttpSession 과 같은 관리 스코프에 둬서도 안됨
- HTTP 요청을 받을때마다 만들고 응답을 리턴할때마다 SqlSession 을 닫아야함

```java
public class SqlMapClient {
	private static SqlSessionFactory sqlsessionfactory;
	static {
		String resource = "SqlMapConfig.xml";
		try {
			 Reader reader = Resources.getResourceAsReader(resource);
			 sqlsessionfactory = new SqlSessionFactoryBuilder().build(reader);
			 
		}catch(Exception e) {
		}
	}
	 public static SqlSessionFactory getSqlSession(){
		  return sqlsessionfactory;
	  }	
}
```
- `sqlsessionfactory` : `SqlSessionFactory` 클래스를 static 객체로 선언
- `Resources.getResourceAsReader(resource)` : 설정 파일을 불러옴
- `SqlSessionFactoryBuilder().build(reader)` : Reader에서 읽은 설정을 기반으로 SqlSessionFactory를 생성
- `getSqlSession` : `sqlsessionfactory` 반환

---

### 3. SqlSession의 활용

```java
SqlSessionFactory sqlsession = SqlMapClient.getSqlSession();

SqlSession session= sqlsession.openSession(); //연결 객체 생성

User user = (User)session.selectOne("Emp.getone", "ALLEN");

System.out.println(user.getEmpno());
System.out.println(user.getEname());

session.close();
```

`openSession()`
- SqlSessionFactory를 사용하여 데이터베이스와의 세션을 여는 기능
- SqlSession 인터페이스를 구현하는 객체를 생성 
- 이 세션을 통해 SQL 쿼리를 실행하고, 트랜잭션을 관리
- 매개변수로 `true`를 넣을 경우 `commit`과 `rollback`이 자동

`session.selectOne`
- 단일 결과를 반환하는 SQL 쿼리를 실행
- 첫 번째 매개변수 : MyBatis 매핑 파일에서 찾을 SQL 쿼리의 이름
- 두 번째 매개변수 : SQL 쿼리에 전달할 매개변수

---
<br>

## Ⅲ. 동적 쿼리

### 1. 동적 쿼리란?

- 실행 시점에 SQL 문장이 동적으로 생성되는 기능
- 동적으로 조건을 추가하거나 SQL 문장의 일부를 선택적으로 포함시킬 수 있음

---

### 2. 동적 쿼리 종류

조건문(if)
- 주어진 조건에 따라 SQL 문장의 일부를 포함하거나 제외

```xml
<select id="selectUsers" parameterType="map" resultType="User">
  SELECT * FROM users
  WHERE 1=1
  <if test="name != null">
    AND name = #{name}
  </if>
  <if test="age != null">
    AND age = #{age}
  </if>
</select>
```

반복문 (forEach)
- 컬렉션의 각 항목에 대해 SQL 문장을 반복적으로 실행

```xml
<select id="selectUsersByIds" parameterType="map" resultType="User">
  SELECT * FROM users
  WHERE id IN
  <foreach item="item" index="index" collection="ids" open="(" separator="," close=")">
    #{item}
  </foreach>
</select>
```

조건 선택 (choose, when, otherwise)
- 여러 조건 중 하나를 선택하여 해당하는 SQL 문장 블록을 실행

```xml
<select id="selectUsers" parameterType="map" resultType="User">
  SELECT * FROM users
  <where>
    <choose>
      <when test="orderBy != null and orderBy == 'name'">
        ORDER BY name
      </when>
      <when test="orderBy != null and orderBy == 'age'">
        ORDER BY age
      </when>
      <otherwise>
        ORDER BY id
      </otherwise>
    </choose>
  </where>
</select>
```

---

### 3. Map의 활용

구문 태그의 `parameterType`에 `HashMap` 타입을 활용할 수 있다.

```xml
<select id="selectSearch" parameterType="hashMap" resultType="guest">
  select * from guest
  <if test="column != null">
    where ${column} like '%' || #{search} || '%' 
  </if>
</select>
```

- `$` : 문자 있는 그대로 출력(컬럼명, 테이블명, Like 검색 에 주로 활용)
- `#` : 따옴표를 붙여 문자열임을 표시

```java
Map<String, String> map = new HashMap<>(); 
  map.put("column", columnName);
  map.put("search", keyvalue);

session.selectList("GUEST.selectSearch" ,map);
```

---
<br>

## Ⅳ. Spring에서의 MyBatis

### 1. Root Container 설정

SqlMapConfig.xml 파일 없어지고 스프링 설정 파일에서 빈 객체로 생성

아래 두 개의 클래스가 위 작업 처리 기존 SqlSessionFactory 기능을 대체
- `SqlSessionFactoryBean`
- `SqlSessionTemplate`

sqlSession이 싱글톤 패턴으로 변경됨
- `close()` 불필요

```xml
<!-- DB연결 설정 -->
<bean id="driverManagerDataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
 	<property name="driverClassName" value="oracle.jdbc.driver.OracleDriver" />
 	<property name="url" value="jdbc:oracle:thin:@localhost:1521:XE" />
 	<property name="username" value="springuser" />
 	<property name="password" value="1004" />
</bean>

<!-- sqlSessionFactoryBean 생성 -->
<bean id="sqlSessionFactoryBean"  class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="driverManagerDataSource"></property>
    <!-- 이 부분만 거의 변한다고 생각하면 된다 -->
    <property name="mapperLocations" value="classpath*:kr/co/mycom/model/mapper/*xml" />
</bean> 

<!-- sqlSession 생성 -->
<bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
  <constructor-arg index="0" ref="sqlSessionFactoryBean"/>
</bean>
```

---

### 2. DAO 인터페이스 구성

- DAO를 인터페이스로 선언 
- 추상 메서드 선언 → Mapper 파일에서 기능 구현

```java
public interface BoardDAO {

 List<BoardDTO> getBoardList(HashMap map);  
 BoardDTO getBoard(int num);                
 int insertBoard(BoardDTO dto);
 int updateBoard(BoardDTO dto);
 int deleteBoard(BoardDTO dto); 
 
}
```

---

### 3. Mapper 구성

1. `mapper`태그의 `namespace`를 DAO를 가지는 interface 이름과 같게 해야한다
2. 구문 태그의 `id`를 interface 가지는 메서드명과 같게 해야 한다.

```xml
<mapper namespace="kr.co.mycom.model.BoardDAO">
  <!-- insertBoard(글 입력하기) -->
  <insert id="insertBoard" parameterType="kr.co.mycom.model.BoardDTO">
    insert into board(num,
                      name,
                      email,
                      pwd,
                      subject,
                      content,
                      regdate,
                      hit,
                      parent,
                      sort,
                      tab)
      values(
          (select nvl(max(num),0)+1 from board),
              #{name},
              #{email},
              #{pwd},
              #{subject},
              #{content},
              sysdate,
              0,
              (select nvl(max(num),0)+1 from board),
              0,
              0
            )
  </insert>

  <!-- updateBoard(글수정하기)  -->
  <update id="updateBoard" parameterType="kr.co.mycom.model.BoardDTO">
    update board set name=#{name} , email=#{email} , subject=#{subject} ,
                    content=#{content} , regdate=sysdate
    where num=#{num} and pwd=#{pwd} 
  </update>

  <!-- deleteBoard(글삭제하기) -->
  <delete id="deleteBoard" parameterType="kr.co.mycom.model.BoardDTO">
    delete from board where num=#{num} and pwd=#{pwd}
  </delete>

  <!-- getBoardList(전체조회) -->
  <select id="getBoardList" parameterType="hashmap" 
                            resultType="kr.co.mycom.model.BoardDTO">
    select * from 
    (
      select A.* ,ROWNUM r from (
                select * from board order by parent desc , sort 
                                ) A
        
    ) where r >= #{start} and r &lt;= #{end}            
  </select>

  <!-- getBoard (글상세보기) -->
  <select id="getBoard" parameterType="Integer" resultType="kr.co.mycom.model.BoardDTO">
    select * from board where num=#{num}
  </select>
</mapper>
```