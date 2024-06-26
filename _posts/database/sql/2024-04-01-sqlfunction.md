---
title: SQL - 함수 (Oracle DB)
date: 2024-04-01 10:30:00 +09:00
categories: [DB, sql]
tags:
  [
    Data,
    DB,
    Datebase,
    oracle,
    sql,
  ]
---

## 1. 문자열 함수

### initcap

문자열의 첫 글자를 대문자로 바꿈

```sql
SELECT initcap('the') FROM dual;
--- The
```

<hr>

### lower

문자열의 모든 글자를 소문자로 변경

```sql
SELECT lower('ABCDE') FROM dual;
-- abcde

SELECT ename, lower(ename) as "lower ename" FROM emp;
/*
SCOTT	scott
ALLEN	allen
WARD	ward
JONES	jones
MARTIN	martin
BLAKE	blake
CLARK	clark
SCOTT	scott
KING	king
TURNER	turner
ADAMS	adams
JAMES	james
FORD	ford
MILLER	miller
*/
```

<hr>

### upper

문자열의 모든 글자를 대문자로 변경

```sql
SELECT upper('abcde') FROM dual;
--ABCDE
```

<hr>

### length

문자열의 글자 개수를 반환

```sql
SELECT length('abcd') FROM dual;
--- 4
```

바이트의 크기가 아닌 글자의 개수이므로 한글이어도 적용 가능

```sql
SELECT length('홍길동') FROM dual;
--3
```

<hr>

### concat

두 문자열을 하나의 문자열로 합침

```sql
SELECT concat('a','b') FROM dual;
--ab

SELECT concat(ename, job) FROM emp;
/*
SCOTTANALYST
ALLENSALESMAN
WARDSALESMAN
JONESMANAGER
MARTINSALESMAN
BLAKEMANAGER
CLARKMANAGER
SCOTTANALYST
KINGPRESIDENT
TURNERSALESMAN
ADAMSCLERK
JAMESCLERK
FORDANALYST
MILLERCLERK
*/
```

문자열 합산 연산자와 역할이 비슷

```sql
SELECT 'a' || 'b' || 'c' FROM  dual;
--abc
```

<hr>

### substr

부분문자열을 추출
- `SUBSTR(문자열, 시작 문자열, 추출할 글자 개수)`
- ORACLE SQL에선 문자열 시작 인덱스가 1임을 기억하자

```sql
SELECT substr('ABCDE', 2, 3) FROM dual;
--BCD
```

세 번째 매개변수를 생략하면 시작 인덱스부터 문자열의 끝까지 추출한다.

```sql
SELECT lower(substr(ename,1,1)) || ' ' || substr(ename, 2) FROM emp;

/*
s COTT
a LLEN
w ARD
j ONES
m ARTIN
b LAKE
c LARK
s COTT
k ING
t URNER
a DAMS
j AMES
f ORD
m ILLER
*/
```

<hr>

### lpad

지정한 글자 수 만큼 왼쪽 문자열을 채움
- `LPAD(표시할 문자열, 총 문자열의 길이, 빈 문자열을 채울 글자)`

```sql
SELECT lpad('ABC', 10, '*') FROM dual;
-- *******ABC
```

<hr>

### rpad

지정한 글자 수 만큼 왼쪽 문자열을 채움
- `RPAD(표시할 문자열, 총 문자열의 길이, 빈 문자열을 채울 글자)`

```sql
SELECT rpad('ABC', 10, '*') FROM dual;
-- ABC*******
```

<hr>

### ltrim

왼쪽의 지정한 문자열을 삭제
- 중복값도 같이 삭제

```sql
SELECT ltrim('MILLLLLLLLER', 'MIL') FROM DUAL;
-- ER
```

<hr>

### rtrim

오른쪽의 지정한 문자열을 삭제

```sql
SELECT rtrim('MILLER', 'ER') FROM DUAL;
-- MILL
```

<hr>

### replace

문자열에서 지정한 부분에 속해있다면 교체함
- `REPLACE(문자열, 교체할 부분, 대체할 문자열)`

```sql
SELECT ename, replace(ename, 'A', '와우') FROM EMP;

/*
SCOTT	SCOTT
ALLEN	와우LLEN
WARD	W와우RD
JONES	JONES
MARTIN	M와우RTIN
BLAKE	BL와우KE
CLARK	CL와우RK
SCOTT	SCOTT
KING	KING
TURNER	TURNER
ADAMS	와우D와우MS
JAMES	J와우MES
FORD	FORD
MILLER	MILLER
*/
```

<hr><br>

## 2. 숫자 함수

### round

반올림 함수
- `round(숫자, 나타낼 자리수)`
- 나타낼 자리수까지 반올림하여 나타냄(0 : 정수부분만 나타냄)
- 두 번째 매개변수의 디폴트값은 0

```sql
SELECT round(12.345) AS r FROM DUAL;
-- 12
SELECT round(12.545) AS r FROM DUAL;
-- 13

SELECT round(12.345, 1) AS r FROM DUAL;
-- 12.3
SELECT round(12.355, 1) AS r FROM DUAL;
-- 12.4

SELECT round(12.345, -1) AS r FROM DUAL;
-- 10
SELECT round(15.355, -1) AS r FROM DUAL;
-- 20
```

<hr>

### trunc

절삭 함수
- `trunc(숫자, 나타낼 자리수)`
- 나타낼 자리수까지만 잘라서 나타냄(0 : 정수부분만 나타냄)
- 두 번째 매개변수의 디폴트값은 0

```sql
SELECT trunc(12.345) AS r FROM DUAL;
-- 12
SELECT trunc(12.545) AS r FROM DUAL;
-- 12

SELECT trunc(12.345, 1) AS r FROM DUAL;
-- 12.3
SELECT trunc(12.355, 1) AS r FROM DUAL;
-- 12.3

SELECT trunc(12.345, -1) AS r FROM DUAL;
-- 10
SELECT trunc(15.355, -1) AS r FROM DUAL;
-- 10
```

<hr>

### mod

나머지 함수
- `MOD(분자, 분모)`
- 분자 / 분모의 나머지를 반환

```sql
SELECT mod(12,10) FROM DUAL;
-- 2

SELECT mod(0,0) FROM DUAL;
-- 0
```

<hr><br>

## 3. 날짜 함수

### 날짜 연산의 특징

날짜와 날짜 혹은 날짜와 숫자 사이 연산 결과 타입은 다음과 같다.

- Date + number >> Date
- Date - number >> Date
- Date - Date >> number

<hr>

### months_between

두 날짜 사이에 월수를 계산하는 함수
- `months_between(날짜1, 날짜2)`
- 실수로 표현됨 >> `TRUNC` 혹은 `ROUND` 활용

```sql
SELECT ROUND(months_between(sysdate, hiredate)) AS "근속 월수" FROM EMP;

/*
498
517
517
516
510
516
514
498
508
511
495
510
510
506
*/
```

<hr><br>

## 4. 변환 함수

### to_char

문자열 변환 함수
- 출력 형식을 정의(포매팅)하기 위해 사용 

```sql
SELECT 
sysdate, 
to_char(sysdate, 'YYYY') || '년' AS yyyy,
to_char(sysdate, 'YEAR') AS year,
to_char(sysdate, 'MM') AS mm,
to_char(sysdate, 'DD') AS dd,
to_char(sysdate, 'DAY')
FROM DUAL;

-- 2024-04-01 11:50:14	2024년	TWENTY TWENTY-FOUR	04	01	월요일
```

<hr>

### to_date

날짜 형식의 문자열을 날짜 타입으로 변경

```sql
SELECT '2024-01-01' + 100 FROM DUAL;
-- 문자열로 인식하여 오류 발생

SELECT to_date('2024-01-01') + 100 FROM DUAL;
-- 2024-04-10 00:00:00
```

<hr>

### to_number

숫자 형식의 문자열을 숫자로
- 자동 형변환때문에 거의 쓰이지 않음

```sql
SELECT '100' + 100 FROM DUAL;
-- 200

SELECT to_number('100') + 100 FROM DUAL;
-- 200
```

<hr><br>

## 5. 일반 함수

### SQL의 단점

> SQL : 구조화된 질의를 할 수 있는 언어

변수와 제어문을 사용할 수 없는 단점이 존재한다.

이를 보완한 것이 PL-SQL
- 변수, 제어문 사용 가능
- 고급기능 존재(커서, 트리거, 함수, 프로시저)

하지만 SQL 안에도 단점을 보완해줄 함수들이 여럿 존재한다.

### nvl

`null`값을 대체하는 역할을 함

```sql
SELECT comm, nvl(comm,0) FROM EMP;

/*
	0
300	300
200	200
30	30
300	300
	0
	0
	0
3500	3500
0	0
	0
	0
	0
	0
*/
```

<hr>

### decode

java의 `if`문과 비슷한 역할
- `decode(칼럼명, 조건값1, 값1, 조건값2, 값2, ... , 값3)`
- 컬럼의 값이 조건값1일 때 값1을 조건값2일 때 값3를 출력하는 식
- 맨 마지막 값은 `else`와 같은 역할 -> 그 외의 경우 값3 출력

```sql
SELECT id, decode(id, 100, '아이티', 200, '영업팀', 300, '관리팀', '기타부서') AS "부서이름" FROM T_EMP;

/*
100	아이티
200	영업팀
300	관리팀
400	기타부서
500	기타부서
*/
```

첫번째 매개변수에 또다른 함수값이 들어올 수 있다.

```sql
SELECT id, decode(substr(jumin,1,1), '1', '남성', '2', '여성', '3', '중성', '기타') AS "성별" FROM T_EMP2;

/*
1	남성
2	여성
3	남성
4	중성
5	기타
*/
```

`decode`안에 `decode`를 재차 사용함으로써 중복 `if`문을 구현할 수 있다. 

```sql
SELECT decode(deptno, 20, decode(ename, 'SMITH', 'HELLO', 'WORLD'), 'ETC') FROM EMP;

/*
WORLD
ETC
ETC
WORLD
ETC
ETC
ETC
WORLD
ETC
ETC
WORLD
ETC
WORLD
ETC
*/
```

<hr>

### case

java의 `switch`문의 역할을 함
- `case 칼럼명 when 조건값1 then 값1 when 조건값2 then 값2 else 값3 end 별칭`
- 칼럼의 값이 조건1일 때 값1을 조건2일 때 값2를 그 외의 경우엔 값3를 출력하는 형식

```sql
SELECT '0' || to_char(zipcode), 
case zipcode when 2 then '서울' when 31 then '경기' when 41 then '제주' else '기타지역' end "지역이름"
FROM t_zip;

/*
02	서울
031	경기
032	기타지역
041	제주
*/
```

조건식이 필요한 경우엔 다음과 같이 사용한다.
- `case when 조건식1 then 값1 when 조건식2 then 값2 else 값3 end 별칭`
- 조건식1이 참이면 값1을 조건식2가 참이면 값2를 그 외의 경우엔 값3을 출력하는 형식

```sql
SELECT case when sal <= 1000 then '4급'
            when sal between 1001 and 2000 then '3급'
            when sal between 2001 and 3000 then '2급'
            when sal between 3001 and 4000 then '1급'
            else '특급' end "급수",
        sal,
        empno,
        ename
FROM emp;

/*
3000	7788	SCOTT
1600	7499	ALLEN
1250	7521	WARD
2975	7566	JONES
1250	7654	MARTIN
2850	7698	BLAKE
2450	7782	CLARK
3000	7788	SCOTT
5000	7839	KING
1500	7844	TURNER
1100	7876	ADAMS
950	7900	JAMES
3000	7902	FORD
1300	7934	MILLER
*/
```

<hr><br>

## 6. 집계함수

### 집계함수의 특성

- 집계함수는 `groub by` 절과 같이 사용
- 모든 집계함수는 `null` 값을 무시
- `select` 절에 집계함수 이외에 다른 컬럼이 오면 반드시 그 컬럼은 groub by 절에 명시되어야한다.
  
<hr>

### count

데이터의 개수를 출력

count(칼럼명) : 데이터 건수(`null` 값 무시)

```sql
SELECT count(comm) FROM EMP;
-- 6
```

`null`도 계산에 넣고 싶다면 `nvl`을 사용

```sql
SELECT count(nvl(comm,0)) FROM EMP
-- 14
```

count(*) : row 수(`null`이 포함된 행도 계산)

```sql
SELECT count(*) FROM EMP;
-- 14
```

<hr>

### sum

컬럼값들의 총합을 출력

```sql
SELECT sal FROM EMP;
/*
3000
1600
1250
2975
1250
2850
2450
3000
5000
1500
1100
950
3000
1300
*/

SELECT sum(sal) FROM emp;
-- 31225
```

<hr>

### avg

칼럼값들의 평균을 계산하여 출력
- ※ null값은 계산에 포함되지 않음

```sql
SELECT trunc(avg(comm)) FROM EMP;
-- 721

-- null 값을 0으로 치환하여 계산에 포함
SELECT trunc(avg(nvl(comm,0))) FROM EMP;
-- 309
```

<hr>

### max

컬럼값 중 최대값을 출력

```sql
SELECT max(sal) FROM EMP;
-- 5000
```

<hr>

### min

컬럼값 중 최소값을 출력

```sql
SELECT min(sal) FROM EMP;
-- 950
```

