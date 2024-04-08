---
title: PL/SQL - CURSOR & PROCEDURE
date: 2024-04-08 11:50:00 +09:00
categories: [DB, pl-sql]
tags:
  [
    Data,
    DB,
    Datebase,
    oracle,
    sql,
    pl-sql,
  ]
---

## Ⅰ. CURSOR

### 1. CURSOR란?

- 행 단위로 데이터를 처리하는 방법을 제공
- 여러건의 데이터를 처리하는 처리하는 방법을 제공

<hr>

### 2. CURSOR 사용법

기본적인 문법은 다음과 같다

```sql
DECLARE
      CURSOR CURSORNAME IS CURSORSTATEMENT  -- 커서가 실행할 문장
BEGIN
      OPEN CURSORNAMEE;  -- 커서가 가지고 있는 문장 실행
          
      FETCH CURSORNAME INTO VAR1, VAR2... -- 커서로부터 데이터를 읽어서 변수에 저장
      CLOSE CURSORNAME -- 커서 닫기
END
```

`CURSOR`와 `LOOP`를 통해 다수의 행에 접근할 수 있따.

```sql
DECLARE
  vempno emp.empno%TYPE;
  vename emp.ename%TYPE;
  vsal   emp.sal%TYPE;
  CURSOR c1  IS select empno,ename,sal from emp where deptno=30;
BEGIN
    OPEN c1; --커서가 가지고 있는 문장 실행
    LOOP  --데이터 row 건수 만큼 반복
      FETCH c1 INTO vempno , vename, vsal;
      EXIT WHEN c1%NOTFOUND; --더이상 row 가 없으면 탈출
        DBMS_OUTPUT.PUT_LINE(vempno || '-' || vename || '-'|| vsal);
    END LOOP;
    CLOSE c1;
END;
```

`FOR`문을 사용하면 더욱 간편하게 표현할 수 있다.
- `OPEN`과 `FETCH` 생략 가능

```sql
DECLARE
  CURSOR emp_curr IS select empno ,ename from emp;
BEGIN
    FOR emp_record IN emp_curr  LOOP --row 단위로 emp_record변수 할당
      EXIT WHEN  emp_curr%NOTFOUND;
      DBMS_OUTPUT.PUT_LINE(emp_record.empno || '-' || emp_record.ename);
    END LOOP;
    CLOSE emp_curr;
END;
```

<hr>

### 3. CURSOR의 속성값들

`CURSORNAME%ROWCOUNT`
- 가장 최근의 SQL 문장에 의해 영향을 받은 행의 수

`CURSORNAME%FOUND` 
- 가장 최근의 SQL 문장이 하나 또는 그 이상의 행에 영향을 미친다면 TRUE 로 평가
  
`CURSORNAME%NOTFOUND` 
- 가장 최근의 SQL 문장이 어떤 행에도 영향을 미치지 않았다면 TRUE 로  평가
  
`CURSORNAME%ISOPEN` 
- PL/SQL 이 실행된 후에 즉시 암시적 커서를 닫기 때문에 항상 FALSE 로 평가

<hr><br>

## Ⅱ. PROCEDURE

### 1. PROCEDURE란?

> 자주 사용되는 쿼리를 모듈화 시켜서 객체로 저장하고 필요한 시점에 호출

- 독자적인 실행이 가능
- 오라클에선 'subprogram'이라고 부름
  
<hr>

### 2. PROCEDURE를 사용하는 이유

편의성 증진
- 복잡한 쿼리문을 간편하게 재사용 가능
  
네트워크 트래픽 감소
- DB 서버로 날리는 SQL구문의 길이가 감소
  
보안 문제 해결
- SQL구문을 도중에 가로채 분석 불가능

### 2. PROCEDURE 생성

생성 문법은 다음과 같다.

```sql
CREATE OR REPLACE PROCEDURE PROCEDURENAME 
IS   
BEGIN
  PROCEDURESTATEMENT;
END;
```

- `CREATE OR REPLACE PROCEDURE` : 생성하거나 덮어쓰기 하겠다는 의미이다.
- `PROCEDURENAME` : 프로시저 이름
- `PROCEDURESTATEMENT` : 프로시저 호출 시 실행될 실행문

생성 시 프로시저가 실행되지는 않는다.

```sql
-- PROCEDURE 예시 
CREATE OR REPLACE PROCEDURE usp_emplist 
IS
BEGIN
  update emp
  set job = 'TTT'
  where deptno=30;
END;
```

<hr>

### 3. PROCEDURE 실행

`EXECUTE`를 통해 만들어둔 프로시저를 호출하여 사용한다.

```sql
EXECUTE PROCEDURENAME;
```

<hr>

### 4. PROCEDURE의 매개변수

input 매개변수를 설정하여 사용자로 부터 입력받게 할 수 있다.

```sql
CREATE OR REPLACE PROCEDURE usp_update_emp
(vempno emp.empno%TYPE)
IS
BEGIN
  update emp
  set sal = 0
  where empno = vempno;
  
END;
```

output 매개변수를 설정하여 결과값을 반환시킬 수 있다.
- `return`이 따로 존재하지 않아 변수에 담아 반환
- `OUT`란 키워드로 매개변수 설정 시 선언해줘야함.
- `IN` 키워드(입력값)는 디폴트 설정

```sql
CREATE OR REPLACE PROCEDUR app_get_emplist
(
  vempno IN emp.empno%TYPE,
  vename OUT emp.ename%TYPE,
  vsal   OUT emp.sal%TYPE
)
IS
BEGIN
  select ename, sal
    into vename , vsal
  from emp
  where empno=vempno;
END;
```

설정한 output은 변수에 따로 저장하여 사용한다.

```sql
DECLARE
  out_ename emp.ename%TYPE;
  out_sal   emp.sal%TYPE;
BEGIN
   app_get_emplist(7902,out_ename,out_sal);  -- 반환값 변수에 할당
   DBMS_OUTPUT.put_line('출력값 : ' || out_ename || '-' || out_sal);
END;
```

<hr>

### 5. CURSOR 타입 매개변수

한 건 이상의 데이터를 `SELECT`한 `CURSOR`를 매개변수에 담기 위해 사용하는 타입

```sql
CREATE OR REPLACE PROCEDURE usp_EmpList
(
  p_sal IN number,
  p_cursor OUT SYS_REFCURSOR 
)
IS
BEGIN
    OPEN p_cursor
    FOR  SELECT empno, ename, sal FROM EMP WHERE sal > p_sal;
END;
```

ORACLE에서 해당 프로시저를 테스트하고 싶다면 다음과 같이 실행한다.

```sql
-- 변수 선언
VAR OUT_CURSOR REFCURSOR; 
-- PROCEDURE를 통해 CURSOR 할당
EXEC usp_EmpList(2000, :OUT_CURSOR); 
-- CURSOR를 출력(모든 데이터 출력)
PRINT OUT_CURSOR;
```
