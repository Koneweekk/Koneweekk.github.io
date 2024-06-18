---
title: Docker - Docker 활용하기
date: 2024-06-18 09:00:00 +09:00
categories: [Server, Docker]
tags:
  [ Server, Docker]
---

sudo docker run -d


## Ⅰ. 컨테이너간 연결

### 1. 데이터베이스 컨테이너 생성

```bash
sudo docker run -d --name wordpressdb -e MYSQL_ROOT_PASSWORD=kosa -e MYSQL_DATABASE=wordpress mysql:5.7
```
`-d`
- 컨테이너를 백그라운드에서 실행

`-e`
- 환경변수 설정. 여기서는 db 비밀번호와 이름을 지정
- 
`--name`
- 컨테이너 이름 지정

`mysql:5.7`
- Docker 이미지 이름과 태그를 지정
- 여기서는 MYSQL 5.7

---

### 2. Wordpress 컨테이너 생성

```bash
sudo docker run -d -e WORDPRESS_DB_PASSWORD=kosa -e WORDPRESS_DB_USER=root --name wordpress --link wordpressdb:mysql -p 80 wordpress
```
`wordpress`
- 사용될 Docker 이미지입니다
- 여기서는 WordPress 이미지를 사용
  
`--link` 
- 두 컨테이너 간에 통신을 가능하게 함
- wordpress 컨테이너에서 mysql이라는 호스트 이름으로 wordpressdb 컨테이너에 접근할 수 있게 함

---

### 3. DB 컨테이너 접속

```bash
sudo docker exec -it wordpressdb /bin/bash
```

`exec`
- 이미 실행 중인 컨테이너 내에서 명령어를 실행
  
`-it`
- 두 개의 옵션이 결합된 형태입니다.
- `-i`(--interactive): 표준 입력(stdin)을 열어둠. 이렇게 하면 사용자 입력을 받을 수 있다.
- `-t `(--tty): 가상 터미널을 할당. 이렇게 하면 터미널과 같은 환경이 제공되어 사용자에게 친숙한 명령줄 인터페이스를 사용 가능

`/bin/bash`
- 컨테이너 내에서 실행할 명령어
- 여기서는 Bash 쉘을 실행하여 사용자가 컨테이너 내부의 Linux 시스템에 접근할 수 있게 함

#### DB 조작

`exec` 명령어 실행 후 컨테이너에 접속하여 mysql db 명령어를 실행할 수 있다.

```bash
mysql -u root -p
```

- `-u root` : MySQL 서버에 접속할 사용자 이름을 지정
- `-p` : MySQL 서버에 접속할 때 비밀번호를 입력받도록 한다.

이후 비밀번호를 입력하고 DB를 조작할 수 있게 된다.

---
<br>

## Ⅱ. 도커 볼륨

### 1. 도커 볼륨이란?

> 도커 컨테이너는 상태를 저장하지 않는다.

위 단점을 해결하기 위해 호스트 파일 시스템의 특정 위치에 데이터를 저장하게 하는 방법
- 컨테이너가 재시작되거나 삭제되더라도 데이터가 지속적으로 유지될 수 있도록 한다.

---

### 2. 호스트 볼륨 공유

```bash
sudo docker run -d --name wordpressdb_hostvolume -e MYSQL_ROOT_PASSWORD=1004 -e MYSQL_DATABASE=wordpress -v /home/wordpress_db:/var/lib/mysql mysql
```

`-v /home/wordpress_db:/var/lib/mysql`
- 볼륨 마운트를 설정하여 호스트의 /home/wordpress_db 디렉터리를 컨테이너의 /var/lib/mysql 디렉터리에 마운트
- 이를 통해 데이터가 컨테이너 외부에 저장되어 컨테이너가 삭제되더라도 데이터는 유지

---

### 3. 볼륨 컨테이너

```bash
sudo docker run -it --name volume-override -v /home/wordpress_db:/home/testdir_2 alicek106/volume_test
```

위 명령어로 컨테이너를 생성 및 실행 후 접속하여 /home/testdir_2 디렉토리에 접속해보면 

```
root@ef8d597920ae:/# ls /home/testdir_2/
#ib_16384_0.dblwr  #innodb_temp   binlog.000002  ca.pem           ib_buffer_pool  mysql       mysql_upgrade_history  public_key.pem   sys       wordpress
#ib_16384_1.dblwr  auto.cnf       binlog.index   client-cert.pem  ibdata1         mysql.ibd   performance_schema     server-cert.pem  undo_001
#innodb_redo       binlog.000001  ca-key.pem     client-key.pem   ibtmp1          mysql.sock  private_key.pem        server-key.pem   undo_002
```

이런 결과가 뜨는 것을 확인할 수 있다.
- 이 결과는 `wordpressdb_hostvolume` 컨테이너의 /var/lib/mysql 디렉토리 구조와 같다.

---

### 4. 특정 컨테이너와 볼륨 공유

```bash
sudo docker run -it --name volumes_from_container --volumes-from volume-override ubuntu
```

`--volumes-from`
- 지정한 컨테이너와 볼륨을 공유

위 명령어로 컨테이너를 생성하여 디렉토리 구조를 탐색하면

```bash
testdir_2  ubuntu
```

testdir_2 디렉토리가 존재하는 것을 확인할 수 있다.
- 즉, 위에서 만든 volume-override 컨테이너와 볼륨이 공유되고 있다.

```bash
'#ib_16384_0.dblwr'  '#innodb_temp'   binlog.000002   ca.pem            ib_buffer_pool   mysql        mysql_upgrade_history   public_key.pem    sys        wordpress
'#ib_16384_1.dblwr'   auto.cnf        binlog.index    client-cert.pem   ibdata1          mysql.ibd    performance_schema      server-cert.pem   undo_001
'#innodb_redo'        binlog.000001   ca-key.pem      client-key.pem    ibtmp1           mysql.sock   private_key.pem         server-key.pem    undo_002
```

testdir_2 디렉토리 내부 또한 똑같다.

---

### 5. 볼륨 직접 생성

```bash
sudo docker volume create --name myvolume
```

위와 같은 명령어로 임의로 볼륨을 생성 가능하다.

```bash
sudo docker run -it --name myvolume_1 -v myvolume:/root/ ubuntu
```

생성한 볼륨은 컨테이너에 마운트 가능하다.

---
<br>

## Ⅲ. 도커 이미지 다루기

### 1. 이미지 조회

`sudo docker images`
- 현재 저장된 도커 이미지들을 조회

`sudo docker inspect wordpress`
- 이미지에 대한 상세 정보 조회

`sudo docker history wordpress`
-  해당 이미지를 구성하는 다양한 레이어 출력
-  출력의 각 줄은 하나의 레이어

---

### 2. 이미지 생성, 저장 및 삭제

#### 생성

`docker commit my-container my-image`
- 컨테이너로부터 이미지 생성
- 컨테이너에 직접 접근하여 파일을 수정한 후 이미지를 추출할 때 사용

#### 삭제

`sudo docker rmi wordpress`
- 이미지를 삭제하는 데 사용

#### 저장

`sudo docker save -o ubuntu.tar ubuntu`
- 이미지를 파일로 저장하는 데 사용
- `ubuntu`라는 이미지를 `ubuntu.tar` 파일로 저장
  
---
<br>


## Ⅳ. 도커 이미지 생성

### 1. Dockerfile을 이용하여 이미지 빌드

`docker build -t my-image .`
- Dockerfile의 내용을 토대로 이미지를 빌드
- `-t` : 생성될 이미지의 이름과 태그를 지정하는 옵션
- `.` : Dockerfile이 위치한 현재 디렉토리를 지정

```
FROM openjdk:17

ARG JAR_FILE=build/libs/*.jar

COPY ${JAR_FILE} app.jar

ENTRYPOINT ["java", "-jar", "app.jar"]
```
- SpringBoot 프로젝트를 빌드하기 위한 Dockerfile의 내용
- build하려는 파일에 따라 내용이 천차만별로 다르니 아래 링크를 참고하여 상황에 맞게 작성
- [Docker File Reference](https://docs.docker.com/reference/dockerfile/)

---

### 2. 도커 레지스트리를 이용

가장 널리 쓰이는 Docker Hub를 이용해보도록 합시다

`docker login`
- 우선 Docker Hub 로그인을 진행

`docker tag my-app:latest {username}/{repo-name}`
- 이미지를 hub에 푸쉬하기 전 Docker Hub에서 사용할 수 있는 형식으로 태그
- `username` : Docker Hub 사용자 이름
- `repo-name` : 레포지토리 이름

`docker push {username}/{repo-name}`
- 이미지 푸쉬

`docker pull {username}/{repo-name}`
- 해당 이미지를 다른 시스템에서 불러올 때 사용

`docker run -d -p {port}:{port} {username}/{repo-name}`
- 설정한 포트에 불러온 이미지를 실행


