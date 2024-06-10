---
title: Tomcat - 이클립스로 Tomcat 웹서버 구동
date: 2024-04-15 10:40:00 +09:00
categories: [Server, Tomcat]
tags:
  [
    Server,
    Eclipse
    Tomcat
  ]
---

## Ⅰ. 이클립스에 Tomcat 서버 등록하기

- Windows > Preferences > Server > Runtime Environment로 접근해서 Add 클릭
  
<img src="/assets/img/post/web/eclipse_tomcat/01.png" width="70%" title="01" alt="01"/> 

- 자신이 설치한 톰캣 버전을 선택 후 Next

<img src="/assets/img/post/web/eclipse_tomcat/02.png" width="70%" title="02" alt="02"/> 

- 자신이 설치한 톰캣 폴더를 선택 후 Finish

<img src="/assets/img/post/web/eclipse_tomcat/07.png" width="70%" title="07" alt="07"/> 

<hr>

## Ⅱ. Tomcat 서버 구동하기

- port 충돌을 피하기 위해 Tomcat 서버의 포트 번호를 변경(반드시 할 필요 X)

<img src="/assets/img/post/web/eclipse_tomcat/03.png" width="70%" title="03" alt="03"/> 
<img src="/assets/img/post/web/eclipse_tomcat/04.png" width="70%" title="04" alt="04"/> 

- 등록된 서버를 구동

<img src="/assets/img/post/web/eclipse_tomcat/05.png" width="100%" title="05" alt="05"/> 

- 아래와 같은 문구가 콘솔창에 출력되면 성공적으로 서버가 구동된 것

<img src="/assets/img/post/web/eclipse_tomcat/06.png" width="70%" title="06" alt="06"/> 
<img src="/assets/img/post/web/eclipse_tomcat/08.png" width="70%" title="08" alt="08"/> 

<hr>

## Ⅲ. 서버로 HTML 파일 배포하기

- Web 프로젝트를 생성

<img src="/assets/img/post/web/eclipse_tomcat/09.png" width="70%" title="09" alt="09"/> 

- 프로젝트 폴더 > src > main > webapp 폴더 하위에 html 파일 생성

<img src="/assets/img/post/web/eclipse_tomcat/10.png" width="100%" title="10" alt="10"/> 

- html 우측 클릭 > Run As Server > Run On Server 선택

<img src="/assets/img/post/web/eclipse_tomcat/11.png" width="70%" title="11" alt="11"/> 

- 등록한 Tomcat 서버를 선택 후 Finish

<img src="/assets/img/post/web/eclipse_tomcat/12.png" width="70%" title="12" alt="12"/> 

- 다음과 같이 html 파일이 서버를 통해 출력됨

<img src="/assets/img/post/web/eclipse_tomcat/13.png" width="70%" title="13" alt="13"/> 