---
title: CI/CD - GitAction과 Docker를 이용한 Vue EC2 자동 배포
date: 2024-06-11 11:35:00 +09:00
categories: [Server, CI/CD]
tags:
  [ Server, CI/CD, Docker, AWS EC2, Vue, Git Action ]
---

Vue의 자동 배포 설정 과정은 아래 포스팅의 내용과 크게 다르지 않다.

[GitAction과 Docker를 이용한 Spring Boot EC2 자동 배포](https://koneweekk.github.io/posts/githubaction_ec2_1/)

차이점은 딱 다음 2가지이다.

1. 프로젝트 빌드 과정
2. Dockerfile 내용

---

## Ⅰ. Vue 프로젝트 빌드

Vue 프로젝트는 npm을 이용하여 패키지를 인스톨하고 빌드한다.

아래는 그 과정을 반영한 gitactions workflow 파일이다.

```yml
name: Project CI/CD

# 동작 조건 설정 : main 브랜치에 push 혹은 pull request가 발생할 경우 동작한다.
on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

permissions:
  contents: read

jobs:
  build-vue:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      # node 세팅
      - uses: actions/setup-node@v3
        with:
          node-version: 16
          cache: npm # setup-node 의 캐시 기능을 사용함
          cache-dependency-path: frontend/package-lock.json # 캐시 기능을 사용할 때 캐시의 기준이 될 파일을 지정
      
      # 모듈 인스톨
      - name: Install Dependencies
        run: |
          cd frontend
          npm install

      # 빌드
      - name: Build with npm
        run: |
          cd frontend
          npm run build
    
      # Docker 이미지 빌드
      - name: docker image build
        run: docker build -t ${{ secrets.DOCKER_USERNAME }}/${{ secrets.DOCKER_FRONTEND_REPO }} frontend

      # DockerHub 로그인
      - name: docker login
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      # Docker Hub 이미지 푸시
      - name: docker Hub push
        run: docker push ${{ secrets.DOCKER_USERNAME }}/${{ secrets.DOCKER_FRONTEND_REPO }}
      
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
            sudo docker stop ${{ secrets.DOCKER_FRONTEND_CONTAINER }}
            sudo docker container rm ${{ secrets.DOCKER_FRONTEND_CONTAINER }}
            sudo docker image rm ${{ secrets.DOCKER_USERNAME }}/${{ secrets.DOCKER_FRONTEND_REPO }}
            sudo docker pull ${{ secrets.DOCKER_USERNAME }}/${{ secrets.DOCKER_FRONTEND_REPO }}
            sudo docker run -d --name ${{ secrets.DOCKER_FRONTEND_CONTAINER }} -p ${{ secrets.DOCKER_FRONTEND_PORT }}:${{ secrets.DOCKER_FRONTEND_PORT }} ${{ secrets.DOCKER_USERNAME }}/${{ secrets.DOCKER_FRONTEND_REPO }}
```

spring boot 설정과 크게 다를게 없고, 아래 3가지만 주의해서 보면 된다.

```yml
# node 세팅
- uses: actions/setup-node@v3
  with:
    node-version: 16
    cache: npm # setup-node 의 캐시 기능을 사용함
    cache-dependency-path: frontend/package-lock.json # 캐시 기능을 사용할 때 캐시의 기준이 될 파일을 지정

# 모듈 인스톨
- name: Install Dependencies
  run: |
    cd frontend
    npm install

# 빌드
- name: Build with npm
  run: |
    cd frontend
    npm run build
```

---

## Ⅱ. Dockerfile

Vue 프로젝트의 Dockerfile은 다음과 같다.

```dockerfile
FROM node:lts-alpine

# install simple http server for serving static content
RUN npm install -g http-server

# make the 'app' folder the current working directory
WORKDIR /app

# copy both 'package.json' and 'package-lock.json' (if available)
COPY package*.json ./

# install project dependencies leaving out dev dependencies
RUN npm install --production

# copy project files and folders to the current working directory (i.e. 'app' folder)
COPY . .

# build app for production with minification
RUN npm run build

EXPOSE 9000
CMD [ "http-server", "dist", "-p", "9000" ]
```

이 때 `EXPOSE`의 포트 번호와 그 밑 `CMD`에 적용한 포트 번호는 같게 해야한다.
- 위 포트 번호는 Docker 컨테이너를 띄운 포트 번호와 동일해야함

