---
title: Linux - WSL 및 Ubuntu 설치하기
date: 2024-04-03 17:50:00 +09:00
categories: [OS, Linux]
tags:
  [
    linux,
    os,
    ubuntu,
    wsl,
  ]
---

## 1. 윈도우 버전 확인

본 포스트는 Windows 10 버전 2004 이상(빌드 19041 이상) 또는 Windows 11에서 WSL2를 쉽게 설치할 수 있는 방법입니다.

우선 윈도우 버전부터 확인합니다.

- 윈도우키 + R을 입력
- winver를 입력하여 윈도우 버전 확인
  
<img src="/assets/img/post/linux/wsl_install/01.png" width="70%" title="01" alt="01"/> 

위 언급한 버전에 해당하면 아래 설치 방법을 따라주시면 되겠습니다.

<hr>

## 2. WSL 설치

- cmd 콘솔을 관리자 권한으로 열어줍니다.
- 콘솔창에 아래 명령어를 입력해줍니다.
  - `wsl --install` 
<img src="/assets/img/post/linux/wsl_install/02.png" width="70%" title="02" alt="02"/> 

- 설치 완료 후 재부팅해줍니다.

<hr>

## 3. 사용자 정보 입력

- 재부팅 후 자동으로 Ubuntu 설치 진행
  
<img src="/assets/img/post/linux/wsl_install/03.png" width="70%" title="03" alt="03"/> 

- 설치 완료후 username 입력

<img src="/assets/img/post/linux/wsl_install/04.png" width="70%" title="04" alt="04"/> 

- password 입력

<img src="/assets/img/post/linux/wsl_install/05.png" width="70%" title="05" alt="05"/> 

- 설치 및 설정 완료

<img src="/assets/img/post/linux/wsl_install/06.png" width="70%" title="06" alt="06"/> 

<hr>

## 4. 설치 완료 확인

- 아래 명령어를 입력하여 배포판 정보 확인
  - `lsb_releaser -a`
  
<img src="/assets/img/post/linux/wsl_install/07.png" width="70%" title="07" alt="07"/> 

위와 같이 출력된다면 설치가 완료된 것