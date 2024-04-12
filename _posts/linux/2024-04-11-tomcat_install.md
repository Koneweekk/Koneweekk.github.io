---
title: Linux - Apache Tomcat 설치하기
date: 2024-04-11 17:50:00 +09:00
categories: [Linux, Install]
tags:
  [
    linux,
    os,
    ubuntu,
    wsl,
    jdk,
    tomcat
  ]
---

## 1. JDK 설치하기

우선 apt를 업데이트

> sudo apt update

<img src="/assets/img/post/linux/tomcat_install/01.png" width="90%" title="01" alt="01"/>  

이후 JDK를 설치합니다(버전은 자유롭게 선택)

> sudo apt install openjdk-11-jdk

<img src="/assets/img/post/linux/tomcat_install/02.png" width="90%" title="02" alt="02"/> 

버전 확인이 된다면 설치 완료

> java -version

<img src="/assets/img/post/linux/tomcat_install/03.png" width="90%" title="03" alt="03"/> 

<hr>

## 2. 톰캣 설치하기

아래 링크로 들어가 tar 파일 다운로드 링크 복사

→ [Apache Tomcat 9 Version Download](https://tomcat.apache.org/download-90.cgi) ←

<img src="/assets/img/post/linux/tomcat_install/14.png" width="90%" title="14" alt="14"/> 

복사한 링크 주소를 통해 아파치 톰캣 tar 파일을 다운로드

> sudo wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.87/bin/apache-tomcat-9.0.87.tar.gz

<img src="/assets/img/post/linux/tomcat_install/04.png" width="90%" title="04" alt="04"/> 

다운로드한 tar 파일 확인

> ls

<img src="/assets/img/post/linux/tomcat_install/05.png" width="90%" title="05" alt="05"/>

tar파일 압축 풀기

> sudo tar -zxvf apache-tomcat-9.0.87.tar.gz

<img src="/assets/img/post/linux/tomcat_install/06.png" width="90%" title="06" alt="06"/>

<hr>

## 3. 톰캣 실행하기

톰캣 폴더로 경로 이동

> cd apache-tomcat-9.0.87/

<img src="/assets/img/post/linux/tomcat_install/07.png" width="90%" title="07" alt="07"/>

톰캣 서버 실행

> sudo ./bin/startup.sh

<img src="/assets/img/post/linux/tomcat_install/08.png" width="90%" title="08" alt="08"/>

ip주소를 확인 후, 서버로 띄운 웹페이지로 이동(포트는 디폴트로 8080)

> ifconfig

<img src="/assets/img/post/linux/tomcat_install/09.png" width="90%" title="09" alt="09"/>

이런 페이지가 뜬다면 서버가 제대로 실행된 것

<img src="/assets/img/post/linux/tomcat_install/10.png" width="90%" title="10" alt="10"/>

직접 만든 html 파일을 서버로 띄울 수도 있다.

<img src="/assets/img/post/linux/tomcat_install/13.png" width="90%" title="13" alt="13"/>

톰캣 종료
> sudo bin/shutdown.sh

<img src="/assets/img/post/linux/tomcat_install/11.png" width="90%" title="11" alt="11"/>



