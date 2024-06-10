---
title: CI/CD - GitAction과 Docker를 이용한 Spring Boot EC2 자동 배포
date: 2024-06-07 12:00:00 +09:00
categories: [Server, CI/CD]
tags:
  [ Server, CI/CD, Docker, AWS EC2, Spring Boot, Git Action ]
---

## Ⅰ. Docker 레포지토리 생성

도커 이미지를 빌드하고 ec2 서버로 가져오기 위해 docker hub 레포지토리를 생성

### 1. 레포지토리 생성 버튼 클릭

![01](/assets/img/post/cicd/docker/docker_githubactions/01.png)

### 2. 레포지토리 이름 입력

![02](/assets/img/post/cicd/docker/docker_githubactions/02.png)

### 3. 레포지토리 생성 학인

![03](/assets/img/post/cicd/docker/docker_githubactions/03.png)

---
<br>

## Ⅰ. EC2 서버 세팅

### 1. EC2 인스턴스 생성

인스턴스 생성은 아래 포스트를 참고

[AWS EC2 인스턴스 생성](https://koneweekk.github.io/posts/ec2_instance/)

---

### 2. Docker 설치

Docker 사용을 위해 서버에 Docker를 설치

- apt-get 업데이트

```
sudo apt-get update
```

- 도커 설치

```
sudo apt-get install docker.io
```

- 도커 실행

```
sudo service docker start
```

---

### 3. 인바운드 포트 열어주기

인스턴스가 활용 중인 보안 그룹으로 접근

![05](/assets/img/post/cicd/docker/docker_githubactions/05.png)

![04](/assets/img/post/cicd/docker/docker_githubactions/04.png)

외부에서 서버에서 접근을 허용해주기 위해 ssh 포트와 스프링부트 실행 포트를 인바운드 포트에 추가

![06](/assets/img/post/cicd/docker/docker_githubactions/06.png)

---
<br>

## Ⅲ. Git Actions 세팅

### 1. 깃액션 설정 yml 생성

루트폴더/.github/workflows 경로에 yml 파일 생성

```yml
{% raw %}
name: Java CI with Gradle

# 동작 조건 설정 : main 브랜치에 push 혹은 pull request가 발생할 경우 동작한다.
on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

permissions:
  contents: read

jobs:
  # Spring Boot 애플리케이션을 빌드하여 도커허브에 푸시하는 과정
  build-docker-image:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3

    # Java 17 세팅
    - name: Set up JDK 17
      uses: actions/setup-java@v3
      with:
        java-version: '17'
        distribution: 'temurin'
    
    # properties 파일 생성
    - name: Make application.properties
      run: |
          cd ./src/main/resources
          touch ./application.properties
          echo "${{ secrets.PROPERTIES }}" > ./application.properties
      shell: bash

    # Spring Boot 애플리케이션 빌드
    - name: Build with Gradle
      uses: gradle/gradle-build-action@67421db6bd0bf253fb4bd25b31ebb98943c375e1
      with:
        arguments: clean bootJar

    # Docker 이미지 빌드
    - name: docker image build
      run: docker build -t ${{ secrets.DOCKER_USERNAME }}/${{ secrets.DOCKER_REPO }} .

    # DockerHub 로그인
    - name: docker login
      uses: docker/login-action@v2
      with:
        username: ${{ secrets.DOCKER_USERNAME }}
        password: ${{ secrets.DOCKER_PASSWORD }}

    # Docker Hub 이미지 푸시
    - name: docker Hub push
      run: docker push ${{ secrets.DOCKER_USERNAME }}/${{ secrets.DOCKER_REPO }}

    # ec2 서버에 도커 컨테이너 실행
    - name: Image pull and run
      uses: appleboy/ssh-action@master
      with:
        host: ${{ secrets.EC2_HOST }}
        username: ${{ secrets.EC2_USERNAME }}
        key: ${{ secrets.EC2_KEY }}
        port: ${{ secrets.EC2_SSH_PORT }}
        script: |
          sudo docker login -u ${{ secrets.DOCKER_USERNAME }} -p ${{ secrets.DOCKER_PASSWORD }}
          sudo docker stop ${{ secrets.DOCKER_CONTAINER }}
          sudo docker container rm ${{ secrets.DOCKER_CONTAINER }}
          sudo docker image rm ${{ secrets.DOCKER_USERNAME }}/${{ secrets.DOCKER_REPO }}
          sudo docker pull ${{ secrets.DOCKER_USERNAME }}/${{ secrets.DOCKER_REPO }}
          sudo docker run -d --name ${{ secrets.DOCKER_CONTAINER }} -p ${{ secrets.DOCKER_PORT }}:${{ secrets.DOCKER_PORT }} ${{ secrets.DOCKER_USERNAME }}/${{ secrets.DOCKER_REPO }}
{% endraw %}
```

push 혹은 pull request시 실행되는 프로세스들을 정의
- 스프링 프로젝트 빌드
- docker 이미지 빌드
- docker hub에 이미지 푸쉬
- ec2 서버에 도커 허브로부터 이미지를 불러와 도커 컨테이너 실행

---

### 2. Git Secret 설정

위 코드의 이중괄호 안에 쓰인 `secrets.변수명`의 값들은 깃헙 SECRET에서 설정을 해줘야한다.

![07](/assets/img/post/cicd/docker/docker_githubactions/07.png)

![08](/assets/img/post/cicd/docker/docker_githubactions/08.png)

#### PROPERTIES

스프링부트의 application.properties 파일의 내용을 말한다.
- 설정 파일의 특성상 보안이 중요한 내용도 추가되므로 secret에 접근하여 파일 생성
- `echo` 명령어는 덮어쓰기이기 때문에 모든 코드를 붙여넣어야한다.

#### DOCKER 관련 설정들

`DOCKER_USERNAME` : Docker 로그인 시 사용자명

`DOCKER_PASSWORD` : Docker 로그인 시 비밀번호

`DOCKER_REPO` : 푸쉬하고자 하는 도커 허브 레포지토리, 아까 생성한 레포지토리 이름을 입력하면 된다.

`DOCKER_CONTAINER` : ec2에서 실행될 도커 컨테이너의 이름

`DOCKER_PORT` : 도커 컨테이너로 접근할 수 있는 포트 번호, 앞에서 인바운드로 열어준 포트번호를 입력

#### EC2 관련 설정

`EC2_HOST` : ec2 인스턴스의 ip주소를 입력

![09](/assets/img/post/cicd/docker/docker_githubactions/09.png)

`EC2_USERNAME` : ec2 인스턴스의 사용자명, AMI로 ubuntu를 선택했다면 기본값은 ubuntu

![10](/assets/img/post/cicd/docker/docker_githubactions/10.png)

`EC2_KEY` : ssh 보안 키를 입력. 다운받은 .pem 파일을 메모장으로 열어서 '모든 내용'을 복붙하면 된다.

`EC2_SSH_PORT` : ssh 연결 포트. 보통은 22 이다.

---

### 3. 배포 확인

이제 변경된 내용을 git push 해보자

![11](/assets/img/post/cicd/docker/docker_githubactions/11.png)

위와 같이 나오면 빌드가 성공된 것이다.

![12](/assets/img/post/cicd/docker/docker_githubactions/12.png)

ec2의 ip주소와 설정한 포트로 접근하면 컨트롤러에 설정해놓은 홈화면이 잘 뜨는 것을 확인할 수 있다.