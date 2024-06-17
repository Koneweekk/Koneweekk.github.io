---
title: Linux - Ubuntu 명령어 1
date: 2024-04-03 18:10:00 +09:00
categories: [OS, Linux]
tags:
  [
    linux,
    os,
    ubuntu,
    wsl,
  ]
---

## 사용자 ID 확인

> whoami

로그인한 사용자의 ID를 알려줍니다.

<img src="/assets/img/post/linux/ubuntu_command/01.png" width="70%" title="01" alt="01"/> 

<hr>

## 현재 디렉토리 출력

> pwd

현재 디렉토리의 위치를 출력합니다.

<img src="/assets/img/post/linux/ubuntu_command/02.png" width="70%" title="02" alt="02"/> 

<hr>

## 디렉토리 목록 출력

> ls

현재 디렉토리 목록을 출력합니다.

<img src="/assets/img/post/linux/ubuntu_command/03.png" width="70%" title="03" alt="03"/> 

> ls -l

현재 디렉토리 목록에 대해 상세히 출력합니다.

<img src="/assets/img/post/linux/ubuntu_command/04.png" width="70%" title="04" alt="04"/> 

> ls -a

현재 디렉토리 목록에서 숨겨진 파일이나 디렉토리까지 출력합니다.

<img src="/assets/img/post/linux/ubuntu_command/05.png" width="70%" title="05" alt="05"/> 

> ls -al

현재 디렉토리 목록에서 숨겨진 파일이나 디렉토리까지 상세히 출력합니다.

<img src="/assets/img/post/linux/ubuntu_command/06.png" width="70%" title="06" alt="06"/> 

<hr>

## 디렉토리 이동

> cd

현재 디렉토리 위치를 입력한 위치로 이동합니다.

<img src="/assets/img/post/linux/ubuntu_command/07.png" width="70%" title="07" alt="07"/> 

<hr>

## 패키지 설치

> apt install 패키지명

ubuntu내 필요한 패키지를 설치합니다.

<img src="/assets/img/post/linux/ubuntu_command/08.png" width="70%" title="08" alt="08"/>

※ 관리자 권한이 없어 `Permission Denied` 에러가 발생

> sudo apt install 패키지명

권리자 권한을 부여한 명령어 수행

<img src="/assets/img/post/linux/ubuntu_command/09.png" width="70%" title="09" alt="09"/>