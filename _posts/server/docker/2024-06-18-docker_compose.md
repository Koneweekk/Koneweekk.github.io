---
title: Docker - Docker Compose
date: 2024-06-19 09:20:00 +09:00
categories: [Server, Docker]
tags:
  [ Server, Docker]
---

## Ⅰ. Docker Compose 개요

### 1. Docker Compose란?

> 다중 컨테이너 도커 애플리케이션을 정의하고 실행하기 위한 도구

- 애플리케이션의 서비스, 네트워크 등을 구성하는 YAML 파일을 작성하여 간편하게 여러 컨테이너를 동시에 실행하고 관리
- 주로 docker-compose.yml 파일을 사용하여 설정을 정의

---

### 2. Docker Compose를 사용하는 이유

#### 다중 컨테이너 애플리케이션 관리

- Docker Compose를 사용하면 하나의 YAML 파일로 여러 컨테이너를 정의하고 구성 가능하다.
- 이는 복잡한 애플리케이션을 쉽게 설정하고 실행할 수 있게 한다.

#### 종속성 관리
- Docker Compose는 컨테이너 간의 종속성을 관리할 수 있다.
- 예를 들어, `depends_on` 옵션을 사용하여 컨테이너들의 실행 순서를 설정할 수 있습니다.

#### 네트워킹 간편화
Docker Compose는 각 서비스 간의 네트워킹을 자동으로 처리한다.
컨테이너들은 서로를 서비스 이름으로 접근할 수 있으며, 복잡한 네트워크 설정 없이도 쉽게 통신할 수 있다.

---
<br>

## Ⅱ. Docker Compose 설치

### 1. Linux

Linux 환경에선 다음과 같은 명령어를 cmd 창에서 입력한다

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/2.27.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

이후 다운로드한 파일에 실행 권한을 부여

```bash
sudo chmod +x /usr/local/bin/docker-compose
```

최신 버전은 아래 링크에서 확인

[GitHub Docker Compose Releases](https://github.com/docker/compose/releases)

---

### 2. Window

윈도우의 경우 Docker Desktop을 설치
- 설치가 완료되면 Docker Compose으로 설치

[Docker Desktop](https://www.docker.com/products/docker-desktop/)

---

### 3. 설치 확인

```bash
docker-compose --version
```

위 명령어를 입력하여 지정한 버전으로 잘 설치되었는지 확인

```bash
Docker Compose version v2.27.1-desktop.1
```

위 처럼 출력된다면 설치 성공한 것

---
<br>

## Ⅲ. Docker Compose 활용

### 1. yaml 파일 작성

만약 Spring Boot와 MySQL 컨테이너를 띄워 상호작용하게 하고 싶다면 다음과 같은 yaml 파일을 작성할 수 있다.

```yaml
version: '3.8'

services:
  mysql:
    image: mysql:8.0
    container_name: mysql_container
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: your_database
      MYSQL_USER: your_user
      MYSQL_PASSWORD: your_password
    ports:
      - "3306:3306"
    volumes:
      - mysql_data:/var/lib/mysql

  springboot-app:
    image: your_springboot_app_image
    container_name: springboot_container
    depends_on:
      - mysql
    environment:
      SPRING_DATASOURCE_URL: jdbc:mysql://mysql:3306/your_database
      SPRING_DATASOURCE_USERNAME: your_user
      SPRING_DATASOURCE_PASSWORD: your_password
    ports:
      - "8080:8080"
    build:
      context: .
      dockerfile: Dockerfile

volumes:
  mysql_data:
```

자세히 살펴보면 다음과 같다.

`version: '3.8'`
- Docker Compose 파일의 버전을 지정

`services:`
- Docker Compose에서 실행할 각 서비스를 정의
- 여기서는 `mysql`과 `springboot-app` 두 개의 서비스를 정의

`image`
- 어떤 이미지를 사용할지 지정
- 여기선 MySQL 8.0의 공식 이미지와 직접 빌드한 Spring Boot 이미지 사용
  
`container_name`
- 생성될 컨테이너의 이름을 설정합니다.
  
`environment` 
- 컨테이너 내에서 사용할 환경 변수를 설정
  
`ports`
- 호스트와 컨테이너 간 포트 매핑을 설정

`volumes`
- 데이터 지속성을 위해 MySQL 데이터베이스 파일을 호스트의 named volume인 mysql_data에 마운트
- 파일 저장 위치는 `/var/lib/mysql`

`depends_on`
- 서비스 시작 순서를 정의
- 여기서는 `springboot-app` 서비스가 시작되기 전에 `mysql` 서비스가 먼저 시작되도록 설정

`build`
Spring Boot 애플리케이션의 Docker 이미지를 빌드할 때 사용할 빌드 컨텍스트와 Dockerfile의 경로를 지정


---

### 2. yaml 파일 실행

docker-compose.yaml 파일이 있는 디렉토리로 이동한 후, 다음 명령을 입력하여 Docker Compose 서비스를 시작
  
```bash
docker-compose up --build
```

`--build`
- Docker 이미지를 다시 빌드하도록 강제
- 이 옵션 없이도 docker-compose up 명령을 실행할 수 있다.
- 애플리케이션 코드나 설정 파일이 변경된 경우 `--build` 옵션을 사용하는 것이 좋다.
