---
title: DB - 반정규화(De-Normalization)
date: 2024-04-17 09:00:00 +09:00
categories: [DB, Modeling]
tags:
  [
    DB,
    Data,
    Datebase,
    Modeling,
    ERD,
    Normalization,
    De-Normalization
  ]
---

## Ⅰ. 반정규란?

### 1. 반정규화의 의미

> 정규화된 데이터 모델을 다시 더 단순한 형태로 변경하는 과정

정규화 : 데이터 중복을 줄이고 데이터 무결성을 보장하기 위해 데이터를 효율적으로 구성하는 과정
- 하지만 성능 상의 이유나 특정한 쿼리 작업을 위해 데이터를 다시 조정해야 하는 경우가 발생

---

### 2. 반정규화의 용도

조인(JOIN) 연산 최적화
- 정규화된 데이터 모델은 여러 테이블에 데이터를 분산시키기 때문에 조인 연산이 많이 발생
- 이러한 조인 연산을 최적화하기 위해 특정 데이터를 하나의 테이블에 결합하여 성능을 향상시킴

쿼리 성능 향상
- 정규화된 데이터 모델은 일부 쿼리 작업에 대해 성능이 저하 발생 가능
- 쿼리 성능 향상을 위해 데이터를 반정규화하여 쿼리 작업을 단순화하는 것이 필요한 경우가 발생

응용 프로그램의 요구 사항
- 특정 응용 프로그램에서는 데이터의 특정 형태나 구조가 필요할 수도 있음
- 이러한 요구 사항을 충족시키기 위해선 데이터를 반정규화하는 것이 유용

---
<br>

## Ⅱ. 반정규화 기법

### 1. 데이터 중복

> 특정 데이터를 여러 테이블에 중복 저장

주문 제품 테이블
- 제품명 컬럼을 추가하여 제품명 조회시 불필요한 join을 삭제

|기존 테이블|반정규화 테이블|
|---|---|
|주문ID|주문ID|
|제품ID|제품ID|
|수량|수량|
||제품명|

---

### 2. 요약 데이터 추가

> 복잡한 집계 연산을 필요로 하는 쿼리의 성능을 향상시키기 위해 요약 데이터를 추가

주문 테이블
- 주문제품 테이블의 데이터로부터 총합 가격을 계산한 컬럼을 추가

|기존 테이블|반정규화 테이블|
|---|---|
|주문ID|주문ID|
|주문날짜|주문날짜|
||총금액|

---

### 3. 이력 컬럼 추가

> 최근인 데이터를 나타내기 위해 이력 컬럼을 추가

등록 차량 테이블
- 등록된 차량 중 최근 등록된 한 대의 차량만 주차장 이용 가능
- 최근 등록 여부 컬럼을 추가하여 불필요한 연산 감소

|기존 테이블|반정규화 테이블|
|---|---|
|차량번호|차량번호|
|연식|연식|
|차종|차종|
|등록일자|등록일자|
|직원번호|최근 등록 여부|
||직원번호|

--- 

### 4. PK 분리 컬럼 추가

> PK값이 여러 의미를 가지고 있다면 PK값을 분리

등록 차량 테이블
- 차량번호엔 지역번호까지 포함됨
- 지역번호를 분리하여 연산을 감소

|기존 테이블|반정규화 테이블|
|---|---|
|차량번호|차량번호|
|연식|연식|
|차종|차종|
|등록일자|등록일자|
|직원번호|직원번호|
||지역번호(차량번호에서 잘라냄)|
