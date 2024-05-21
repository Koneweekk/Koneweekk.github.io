---
title: DATABASE - MySQL DB 설치하기
date: 2024-05-21 16:30:00 +09:00
categories: [DB, DB install]
tags:
  [
    Data,
    DB,
    Datebase,
    MySQL,
    설치
  ]
---

## Ⅰ. MySQL DB 설치

### 1. 실행 파일 설치

[My-SQL 8버전 다운로드](https://dev.mysql.com/downloads/windows/installer/8.0.html) (※ 회원가입 및 로그인 필요)

다운로드 후 파일을 클릭하여 실행해 다운로드를 진행

---

### 2. 다운로드

<img src="/assets/img/post/database/install/mysql_db//01.png" width="100%" title="01" alt="01"/> 

- 커스텀 옵션을 선택하여 원하는 설치 프로그램을 선택
   
<img src="/assets/img/post/database/install/mysql_db//02.png" width="100%" title="02" alt="02"/>  

- DB서버와 Workbench를 선택

<img src="/assets/img/post/database/install/mysql_db//03.png" width="100%" title="03" alt="03"/>

- 선택 후 next, excute를 누르면 설치 진행

<img src="/assets/img/post/database/install/mysql_db//04.png" width="100%" title="04" alt="04"/>  

- 설치가 완료되면 next 누른 후, 환경설정 창으로 이동

---

### 3. 환경설정

<img src="/assets/img/post/database/install/mysql_db//05.png" width="100%" title="05" alt="05"/>

- 서버 컴퓨터 타입과 포트 번호를 입력(별 일 없으면 처음 설정 그대로)

<img src="/assets/img/post/database/install/mysql_db//06.png" width="100%" title="06" alt="06"/>  

- 보안 인증 방식 선택(별 일 없으면 처음 설정 그대로)

<img src="/assets/img/post/database/install/mysql_db//07.png" width="100%" title="07" alt="07"/>  

- 접속 비밀번호 설정(안 잊어버리게 조심)

<img src="/assets/img/post/database/install/mysql_db//08.png" width="100%" title="08" alt="08"/>  

- 서비스 이름 입력

<img src="/assets/img/post/database/install/mysql_db//09.png" width="100%" title="09" alt="09"/>  

- 설정 진행

<img src="/assets/img/post/database/install/mysql_db//10.png" width="100%" title="10" alt="10"/> 

- 설정 완료

---
<br>

## Ⅰ. MySQL DB 접속

### 1. 루트 계정 접속

<img src="/assets/img/post/database/install/mysql_db//11.png" width="100%" title="11" alt="11"/>

- workbench 접속

<img src="/assets/img/post/database/install/mysql_db//12.png" width="100%" title="12" alt="12"/>  

- 비밀번호 입력

<img src="/assets/img/post/database/install/mysql_db//13.png" width="100%" title="13" alt="13"/>  

- `create database shop default character set utf8 collate utf8_general_ci;`
  - `shop`이란 테이블을 생성(한글이 깨지지 않게 인코딩 설정 추가)
- `show databases`
  - 현재 생성된 데이터베이스들을 조회

----

### 2. 추가 계정 생성

<img src="/assets/img/post/database/install/mysql_db//14.png" width="100%" title="14" alt="14"/>  

- 계정 이름과 포트를 설정한 후 추가 계정 생성(비밀번호는 기존 비밀번호)

<img src="/assets/img/post/database/install/mysql_db//15.png" width="100%" title="15" alt="15"/>  

- 계정이 잘 생성된 것을 볼 수 있음
