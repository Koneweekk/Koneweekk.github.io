---
title: Docker - Docker Desktop 설치
date: 2024-06-07 09:20:00 +09:00
categories: [CI/CD, Docker]
tags:
  [ CI/CD, Docker]
---

## Ⅰ. WSL 설치

설치 방법은 아래 포스팅 참고

[WSL 설치](https://koneweekk.github.io/posts/wsl_install/)

- WSL : Windows에서 Linux 배포판을 네이티브로 실행할 수 있게 해주는 호환성 계층
- Docker : Linux 기반의 컨테이너를 활용하여 응용 프로그램을 격리된 환경에서 실행할 수 있게 해주는 도구

Docker Desktop은 WSL 2와의 통합을 지원하여 더 나은 성능과 사용성을 제공

---

## Ⅱ. Docker Desktop 설치

아래 홈페이지에서 Docker Desktop 설치파일 다운로드

[Docket Desktop](https://www.docker.com/products/docker-desktop/)

![설치0](/assets/img/post/cicd/docker/docker_install/00.png)

다운로드가 완료되면 설치파일 실행
- 위에서 WSL을 설치했으니 그대로 진행 

![설치1](/assets/img/post/cicd/docker/docker_install/01.png)

설치가 시작되면 아래와 같은 화면이 나타난다.

![설치2](/assets/img/post/cicd/docker/docker_install/02.png)

---

## Ⅲ. Docker 세팅

Docker Desktop 홈화면이다. WSL 설정을 위해 상단 톱니바퀴 버튼을 눌러 세팅창으로 이동

![설치4](/assets/img/post/cicd/docker/docker_install/04.png)

General의 'Use the WSL 2 Based engine' 설정이 체크가 되어있지 않다면 체크

![설치5](/assets/img/post/cicd/docker/docker_install/05.png)

Resourced의 'Enable integration with my default WSL distro' 설정이 체크가 되어있지 않다면 체크

![설치6](/assets/img/post/cicd/docker/docker_install/06.png)