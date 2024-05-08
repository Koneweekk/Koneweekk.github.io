---
title: DATABASE - Oracle DB 설치하기
date: 2024-03-29 11:00:00 +09:00
categories: [DB, DB install]
tags:
  [
    Data,
    DB,
    Datebase,
    oracle,
    설치
  ]
---

## Ⅰ. Oracle 데이터베이스 설치

### 1. 프로그램 다운로드

[오라클 11gR2버전 다운로드](https://www.oracle.com/database/technologies/xe-prior-release-downloads.html) (※ 회원가입 및 로그인 필요)

<img src="/assets/img/post/database/install/oracle_db/01.png" width="100%" title="01" alt="01"/>  

자신의 OS에 맞는 버전에 맞게 버전을 선택

<hr>

### 2. 프로그램 설치

1 : 다운로드 폴더 압축 풀기
2 : DISK 폴대 내 setup.exe 파일 실행

<img src="/assets/img/post/database/install/oracle_db/02.png" width="100%" title="02" alt="02"/>  

3 : Next 클릭

<img src="/assets/img/post/database/install/oracle_db/03.png" width="70%" title="03" alt="03"/>  

4 : 라이센스 동의 후 Next 클릭

<img src="/assets/img/post/database/install/oracle_db/04.png" width="70%" title="04" alt="04"/>  

5 : 설치 경로 지정. 바꾸지 않는 걸 추천

<img src="/assets/img/post/database/install/oracle_db/05.png" width="70%" title="05" alt="05"/>  

6 : 관리 계정 비밀번호 설정

<img src="/assets/img/post/database/install/oracle_db/06.png" width="70%" title="06" alt="06"/>  

7 : install을 눌러 설치 진행

<img src="/assets/img/post/database/install/oracle_db/07.png" width="70%" title="07" alt="07"/>  

8 : 설치가 완료되면 Finish 클릭

<img src="/assets/img/post/database/install/oracle_db/08.png" width="70%" title="08" alt="08"/>  

<hr>

### 3. 설치 확인

1 : cmd창에 `sqlplus` 입력

<img src="/assets/img/post/database/install/oracle_db/09.png" width="100%" title="09" alt="09"/>  

2 : user-name 에 `SYSTEM`, password 에는 설치 시에 입력한 패스워드 입력

<img src="/assets/img/post/database/install/oracle_db/10.png" width="100%" title="10" alt="10"/>  

3 : `select * from all_users`를 입력 시 다음과 같이 출력된다면 정상 설치된 것

<img src="/assets/img/post/database/install/oracle_db/11.png" width="100%" title="11" alt="11"/>  

<hr><br>

## Ⅱ. Oracle SQL Developer 설치

### 1. 프로그램 다운로드

[Oracle SQL Developer 다운로드](https://www.oracle.com/database/sqldeveloper/technologies/download/) (※ 회원가입 및 로그인 필요)

<img src="/assets/img/post/database/install/oracle_db/12.png" width="100%" title="12" alt="12"/>  

자신의 OS에 맞는 버전에 맞게 버전을 선택

만약 jdk가 설치되어 있지 않다면 with jdk 문구가 달린 버전을 설치

<hr>

### 2. 프로그램 실행

1 : 파일 압축 해제 후 sqldeveloper.exe 란 파일 실행

<img src="/assets/img/post/database/install/oracle_db/13.png" width="100%" title="13" alt="13"/>  

2 : 접속 생성 버튼 클릭

<img src="/assets/img/post/database/install/oracle_db/14.png" width="100%" title="14" alt="14"/>  

3 : 루트 사용자 접속 생성
- Name : `SYSTEM` 입력
- 사용자 이름 : 아무렇게나 입력
- 비밀번호 : 설치시 설정한 비밀 번호 입력

<img src="/assets/img/post/database/install/oracle_db/15.png" width="100%" title="15" alt="15"/>  

4 : 접속된 것을 확인

<img src="/assets/img/post/database/install/oracle_db/16.png" width="100%" title="16" alt="16"/>  

<hr>

### 3. 사용자 추가

1 : SYSTEM의 다른 사용자에서 우측 클릭 후 사용자 생성 클릭

<img src="/assets/img/post/database/install/oracle_db/17.png" width="70%" title="17" alt="17"/>  

2 : 사용자 이름과 패스워드를 설정
- 테이블 스페이스는 그림과 같이 설정
  
<img src="/assets/img/post/database/install/oracle_db/18.png" width="70%" title="18" alt="18"/>  

3 : 부여된 롤에서 CONNECT와 RESOURCE 부분 모두 체크 후 적용

<img src="/assets/img/post/database/install/oracle_db/19.png" width="70%" title="19" alt="19"/> 

4 : 새 사용자 추가 후 입력값 입력 후 접속
- Name : 등록된 사용자 이름
- 사용자 이름 : 아무렇게나 입력
- 비밀번호 : 사용자 설정 시 등록한 비밀번호
<img src="/assets/img/post/database/install/oracle_db/20.png" width="100%" title="20" alt="20"/> 

