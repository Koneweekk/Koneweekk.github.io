---
title: SQL - CONSTRAINT (Oracle DB)
date: 2024-04-03 11:30:00 +09:00
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

## 1. CONSTRAINT(제약 조건)이란?

### CONSTRAINT의 의미

> 테이블에 적용되는 규칙이나 조건

- 테이블에 문제되거나 결함있는 데이터가 입력되지 않도록 컬럼별로 미리 지정해 둔 조건
- 데이터 무결성을 보장하고 데이터베이스의 일관성을 유지하는 데 중요한 역할을 함

CONSTRAINTS은 다음과 같은 시기에 추가된다

1. 테이블 생성시
2. 테이블 수정시(추천)

<hr>

### CONSTRAINT의 종류

PRIMARY KEY(PK)
- 테이블에서 유일한 값을 식별하기 위해 사용되는 열 또는 열의 집합을 지정

FOREIGN KEY(FK)
- 테이블 간의 관계를 유지하기 위해 사용
- 한 테이블의 열이 다른 테이블의 기본 키를 참조하는 데 사용

UNIQUE
- 테이블 내에서 중복된 값을 허용하지 않는 열에 적용

NOT NULL
- 특정 열이 NULL 값을 허용하지 않도록 지정

CHECK
- 열에 저장될 수 있는 값의 범위나 형식을 지정

<hr><br>

## 2. PRIMARY KEY(PK)

### 특징

> 테이블에서 유일한 값을 식별하기 위해 사용되는 열 또는 열의 집합을 지정

- `NOT NULL`과 `UNIQUE` 조건을 만족
- 테이블당 1개 지정 가능
- 복합키 사용 가능(다수 컬럼을 조합하여 데이터 식별)

<hr>

### 사용법

`CONSTRAINT 제약이름 PRIMARY KEY,`
- `ID NUMBER PRIMARY KEY` 와 같은 방식은 추천하지 않는다.
- 제약 이름을 오라클 서버에서 임의로 정해버리기 때문

```sql
create table temp7(
   id number constraint pk_temp7_id primary key,
   name varchar2(20) not null,
   addr varchar2(50)
);

insert into temp7(id, name, addr) values(1, '홍길동', '강남구');
insert into temp7(id, name, addr) values(1, '중복이', '강남구');
-- ORA-00001: unique constraint (KOSA.PK_TEMP7_ID) violated
```

테이블 수정 시 제약을 걸고 싶다면 다음과 같이 사용한다.

```sql
alter table temp7
add constraint pk_temp7_id primary key(id);
```

복합키는 테이블 생성시엔 설정하지 못 한다.
- 수정시에만 설정할 수 있음

```sql
alter table temp7
add constraint pk_temp7_id primary key(id, addr);
```

<hr>

### ※ INDEX

> 특정 열의 값을 미리 정렬하고 특정 값을 빠르게 찾을 수 있는 포인터를 제공

칼럼에 `PRIMARY KEY`조건을 걸면 자동으로 index가 설정됨

index 존재 X
- 원하는 데이터를 찾기 위해 전부 스캔
- 최악의 경우 데이터 전부를 스캔해야할 수도 있음

index 존재 O
- index에 맞는 데이터를 바로 찾아가게됨
- 검색 속도 향상

이러한 이유로 pk값으로 데이터를 검색하는 것이 속도가 빠름
- 하지만 무분별한 index의 사용은 성능을 오히려 저하시킴
  
<hr><br>

## 3. FOREIGN KEY(FK)

### 특징

> 테이블 간의 관계를 유지하기 위해 사용  
> 한 테이블의 열이 다른 테이블의 기본 키를 참조하는 데 사용

- 참조하려는 컬럼이 테이블의 pk이어야 제약 지정 가능

<hr>

### 사용법

`CONSTRAINT 제약명 REFERENCES 참조테이블(참조키)`

```sql
create table c_emp2(
    empno number constraint pk_c_emp2_empno primary key,
    deptno number constraint fk_c_emp2_deptno references c_dept(deptno)
);
```

`CONSTRAINT 제약명 FOREIGN KEY(참조 컬럼) REFERENCES 참조테이블(참조키)`

```sql
alter table c_emp
add constraint fk_c_emp_deptno foreign key(deptno) references c_dept(deptno);
```

참조되고 있는 데이터는 삭제할 수 없다.

```sql
insert into c_emp(empno, ename, deptno) values(1, '홍길동', 100);

delete from c_dept where deptno=100;
-- ORA-02292: integrity constraint (KOSA.FK_C_EMP_DEPTNO) violated - child record found
```

만약 참조되고 있는 데이터를 삭제하고 싶다면 참조하는 데이터부터 삭제해야한다.

```sql
delete from c_emp where deptno=100;
```

혹은 `CASCADE` 옵션을 설정 → 참조하는 데이터들이 자동으로 함께 삭제

```sql
-- CONSTRAINT 삭제
alter table c_emp
drop constraint fk_c_emp_deptno;

-- CASCADE 설정된 FOREIGN KEY CONSTRAINT 추가 
alter table c_emp
add constraint fk_c_emp_deptno foreign key(deptno) references c_dept(deptno) on delete cascade;
```

<hr><br>

## 4. UNIQUE

### 특징

> 테이블 내에서 중복된 값을 허용하지 않는 열에 적용

- 테이블의 컬럼 개수 만큼 제약을 걸 수 있다.
- 디폴트로 `NULL` 허용 (`NOT NULL` 설정 가능)

<hr>

### 사용법

`CONSTRAINT 제약이름 PRIMARY KEY`

```sql
create table temp8 (
  id number constraint pk_temp8_id primary key,
  name varchar2(20) not null,
  jumin nvarchar2(6) constraint uk_temp8_jumin unique,
  addr varchar2(50)
)

insert into temp8(id, name, jumin, addr) values(1, '홍길동', '123456', '경기도');
insert into temp8(id, name, jumin, addr) values(2, '아무개', '123456', '경기도');
-- ORA-00001: unique constraint (KOSA.UK_TEMP8_JUMIN) violated
```

테이블 수정 시 제약을 걸고 싶다면 다음과 같이 사용한다.

```sql
alter table temp8
add constraint pk_temp7_id unique(jumin);
```

<hr><br>

## 5. NOT NULL

### 특징

> 특정 열이 NULL 값을 허용하지 않도록 지정

- 지정 시 `CONSTRAINT` 키워드를 사용하지 않음

<hr>

### 사용법

```sql
create table temp8 (
  id number constraint pk_temp8_id primary key,
  name varchar2(20) not null,
  jumin nvarchar2(6) constraint uk_temp8_jumin unique,
  addr varchar2(50)
)
```

테이블 수정 시 제약을 걸고 싶다면 다음과 같이 사용한다.
- 다른 제약과는 다르게 `modify`를 사용
- `CONSTRAINT` 불필요

```sql
alter table temp8
modify(name not null);
```

<hr><br>

## 6. CHECK

### 특징

> 열에 저장될 수 있는 값의 범위나 형식을 지정

`WHERE` 사용하는 조건절을 제약으로 사용

<hr>

### 사용법

`CONSTRAINT 제약명 CHECK(조건)`

```sql
create table temp9 (
  id number constraint pk_temp9_id primary key,
  name varchar2(20) not null,
  jumin char(6) not null constraint uk_temp9_jumin unique,
  addr varchar2(30),
  age number constraint ck_temp9_age check(age >= 19)
)

insert into temp9 values(100, '야무지개', '123456', '서울', 18);
-- ORA-02290: check constraint (KOSA.CK_TEMP9_AGE) violated
```


