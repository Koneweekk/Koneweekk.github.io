---
title: 웹 - 웹 스토리지(Web Storage)
date: 2024-04-25 10:20:00 +09:00
categories: [CS, 웹]
tags: [CS, Web, cookie, local storage, session storage]
---

## Ⅰ. 웹 스토리지(Web Storage)

### 1. 웹 스토리지란?

- 웹 브라우저에서 데이터를 임시 또는 영구적으로 저장할 수 있는 클라이언트 측 저장소
- 웹 애플리케이션에서 사용자 정보, 상태, 설정 및 기타 데이터를 저장하고 관리하는 데 사용

---

### 2. 웹 스토리지를 사용하는 이유

상태 유지
- 사용자 상태를 유지하기 위해 데이터를 저장
- 사용자의 로그인 상태, 세션 정보, 또는 사용자가 이전에 수행한 작업 등을 저장

오프라인 지원
- 웹 스토리지를 사용하면 웹 애플리케이션이 오프라인에서도 작동 가능
- 로컬 스토리지를 통해 필요한 데이터를 클라이언트 측에서 저장하고 나중에 동기화

성능 향상
- 클라이언트 측에서 데이터를 저장하면 서버로의 요청을 줄일 수 있음
- 사용자의 환경 설정이나 일부 캐시 데이터를 로컬 스토리지에 저장하여 애플리케이션의 성능을 향상 가능

---
<br>

## Ⅱ. 웹 스토리지의 종류

### 1. 쿠키(Cookie)

- 로컬 디스크에 txt 암호화 시켜서 저장한다.
- 주로 서버에 요청 시 HTTP 헤더에 포함되어 전송된다.
- 용량 제한(4096Kb)이 있다.
- 보안적 취약성(사용자가 삭제 가능, 해커가 접근 가능)이 존재한다.
  
메모리 쿠키(웹브라우저)
- 클라이언트가 강제로 지우지 않는 한 브라우저 닫기 전까지 유효
	
파일 쿠키
- Client가 강제로 지우지 않는 한 정해진 시간까지 유효

---

### 2. 로컬 스토리지(Local Storage)

- 사용자 브라우저에 데이터를 저장
- 클라이언트 측에서 데이터를 영구적으로 저장하는 데 사용
- 대용량 데이터 저장이 가능하며, 보안 문제도 쿠키보다는 적음
- JSON 형태로 read , write
- 서버와의 통신 없이 사용 가능
- 브라우저의 도메인(Origin) 단위로 관리

---

### 3. 세션 스토리지(Session Storage)

- 세션 동안만 데이터를 저장할 수 있는 클라이언트 측 스토리지
- 브라우저 세션이 종료되면 데이터가 삭제
- 새로고침을 하거나 탭을 닫으면 데이터가 손실
- 주로 한 세션 내에서 데이터를 유지하고 싶을 때 사용
- 탭 별로 관리됨

---
<br>

## Ⅲ. 세션(Session)

### 1. 세션이란?

- 웹 서버와 클라이언트 간의 특정 상호작용을 나타내는 일련의 요청과 응답을 포함하는 상태
- 보통은 사용자가 웹 애플리케이션에 로그인한 후에 시작
- 로그아웃하거나 일정 시간이 지나면 종료됩니다.

---

### 2. 세션의 특징

유지
- 세션은 일정 시간 동안 유지
- 사용자가 로그인한 후 일정 시간(설정하지 않으면 브라우저 종료 시) 동안 세션은 계속 유지
- 사용자가 로그아웃, 브라우저 종료, 세션 타임아웃이 발생하면 종료

상태 유지
- 세션은 서버 측에서 상태 정보를 유지
- 각 클라이언트의 상태 정보를 저장하고 관리
- 사용자의 상호작용에 따라 애플리케이션의 동작을 조정
  
고유성
- 각 세션은 고유한 세션 ID로 식별
- 이 세션 ID는 클라이언트 측에서 유지
- 서버 측에서는 세션 ID를 사용하여 해당 세션에 대한 정보를 식별

---

### 3. 세션의 메커니즘

세션 시작
- 사용자가 웹 애플리케이션에 접속하면, 클라이언트는 서버에 요청을 보냄
- 이 요청에는 일반적으로 세션을 시작하는 데 필요한 정보가 포함
  
세션 식별
- 서버는 클라이언트의 요청을 받고, 새로운 세션을 시작하거나 기존 세션을 식별
- 이를 위해 클라이언트에게 고유한 세션 ID를 부여하고, 이 ID를 사용하여 해당 세션을 식별

상태 유지
- 세션이 시작되면, 서버는 해당 세션과 연결된 상태 정보를 유지
- 이 정보는 보통 서버의 메모리나 데이터베이스에 저장
- 사용자의 인증 상태, 세션 변수, 사용자의 장바구니 내용 등이 여기에 포함될 수 있음
  
요청과 응답
- 클라이언트가 서버에 요청을 보내면, 요청에는 클라이언트의 세션 ID가 포함
- 서버는 이 세션 ID를 사용하여 해당 세션과 연결된 상태 정보를 식별하고 처리
  
세션 종료
- 사용자가 로그아웃하거나 세션 타임아웃이 발생하면, 세션은 종료
- 이 때 서버는 해당 세션과 연관된 상태 정보를 정리하고, 세션을 종료