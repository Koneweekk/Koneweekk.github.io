---
title: AWS - EC2 인스턴스 생성 및 세팅
date: 2024-06-07 10:30:00 +09:00
categories: [CI/CD, AWS]
tags:
  [ CI/CD, AWS EC2]
---

## Ⅰ. AWS EC2 인스턴스 생성

### 1. AWS EC2 서비스 화면으로 이동하여 인스턴스 시작 클릭

![01](/assets/img/post/cicd/aws/ec2-instance/01.png)

---

### 2. AMI(Amazon Machine Image) 선택

- 이 글에선 ubuntu를 선택

![02](/assets/img/post/cicd/aws/ec2-instance/02.png)

---

### 3. 인스턴스 유형 선택
- 필요한 성능과 크기에 따라 적절한 유형을 선택
- 프리티어 지원 여부도 잘 확인

![04](/assets/img/post/cicd/aws/ec2-instance/04.png)

---

### 4. 보안그룹 생성

> AWS EC2 인스턴스의 네트워크 트래픽을 제어하는 가상 방화벽 역할

- 기존 보안 그룹은 모든 트래픽을 허용(상태 기반 규칙 사용)
- EC2 인스턴스는 터미널을 통해 접속해야하기 때문에 SSH 22포트가 기본값으로 설정 

![07](/assets/img/post/cicd/aws/ec2-instance/07.png)

---

### 5. 키 페어 생성

이때 프라이빗 키 파일은 한 번 다운로드 이후 다시 받을 수 없기 때문에 잘 간직하자

![05](/assets/img/post/cicd/aws/ec2-instance/05.png)

![06](/assets/img/post/cicd/aws/ec2-instance/06.png)

---

### 6. 인스턴스 생성

모두 완료되었다면 인스턴스 시작

![08](/assets/img/post/cicd/aws/ec2-instance/08.png)

---
<br>

## Ⅱ. 탄력적 IP 세팅

### 1. 탄력적 IP 세팅 이유

인스턴스는 중지하고 새로 시작할 때 마다 IP를 새로 생성한다.
- 이를 방지하기 위해 탄력적 IP 세팅을 통해 고징 IP를 세팅

---

### 2. 탄력적 IP 생성 화면 이동

![09](/assets/img/post/cicd/aws/ec2-instance/09.png)

작업란을 통해 탄력적 IP 주소 연결 탭을 클릭

![10](/assets/img/post/cicd/aws/ec2-instance/10.png)

---

### 3. 탄력적 IP 생성

![12](/assets/img/post/cicd/aws/ec2-instance/12.png)

---

### 4. 인스턴스 연결

탄력적 IP를 연결하고자하는 인스턴스를 선택 후 연결

![11](/assets/img/post/cicd/aws/ec2-instance/11.png)

※ 만약 인스턴스를 연결해주지 않으면 과금되니 주의!