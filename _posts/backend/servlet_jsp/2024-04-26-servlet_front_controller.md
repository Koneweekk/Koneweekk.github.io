---
title: Servlet - 프론트 컨트롤러(Front Controller)
date: 2024-04-26 09:40:00 +09:00
categories: [Back-End, Servelt & JSP]
tags: [Back-End, Servelt, JSP, Front Controller]
---

## Ⅰ. 프론트 컨트롤러(Front Controller) 개요

### 1. 프론트 컨트롤러란?

> 웹 애플리케이션에서 들어오는 모든 요청을 처리하는 중앙 집중식 컨트롤러

- 애플리케이션의 다른 컴포넌트들이 특정 요청에 대한 처리를 담당하기 전에 먼저 실행되는 핵심 컴포넌트

---

### 2. 프론트 컨트롤러의 역할

요청 분석 및 라우팅
- 클라이언트로부터 들어온 HTTP 요청을 분석
- 요청된 작업을 처리할 적절한 컨트롤러 또는 핸들러로 라우팅
  
전처리 작업 처리
- 요청에 대한 전처리 작업을 수행
- 요청으로부터 인증 정보를 확인하거나 로깅을 수행
  
컨트롤러 호출
- 적절한 컨트롤러나 핸들러를 호출
- 실제 요청 처리 작업을 위임
  
후처리 작업 처리
- 요청 처리 이후에는 후처리 작업을 수행
- ex) 모델 데이터를 뷰에 전달하기 전에 추가적인 처리 작업을 수행

---

### 3. 구현 방식의 종류

커맨더 방식
- url의 쿼리 문자열로 지정할 핸들러나 뷰를 지정하는 방식
- `url?cmd=board`, `url?cmd=login`

url 방식
- URL에 따라 각각 다른 핸들러를 호출하여 요청을 처리
- `url/board`, `url/login` 등등


---
<br>

## Ⅱ. 사용법

### 1. 기본적인 순서

1. 한글 처리 : `request.setCharacterEncoding("UTF-8")`를 통해 한글깨짐 방지
2. 요청 받기 : url 혹은 쿼리스트링 요청받기
3. 요청 판단 : 넘어온 요청으로 어떤 로직을 거칠지 판단
4. 결과 저장 : request, session, application 객체 이용
5. view를 지정 : 요청에 알맞는 view 지정
6. buffer에 jsp 내용을 담고 데이터 출력
7. forward를 걸어서 전달

---

### 2. 기본 예시 코드

```java
@WebServlet(description = "게시판 Front Controller", urlPatterns = { "/Board" })
public class FrontBoardController extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
	public FrontBoardController() {
        super();
    }
	
	private void doProcess(HttpServletRequest request, HttpServletResponse response, String method) throws ServletException, IOException {
		// 한글 처리
		request.setCharacterEncoding("UTF-8");
		
		// 요청 받기
		String cmd = request.getParameter("cmd");
		
		// 요청 판단
		String viewpage = null;
		if(cmd == null) {
			viewpage = "/error/error.jsp";
		} else if(cmd.equals("boardlist")) {
			viewpage = "/board/boardlist.jsp";
		} else if(cmd.equals("boardwrite")) {
			viewpage = "/board/boardwrite.jsp";
		} else if(cmd.equals("boarddelete")) {
			viewpage = "/board/boarddelete.jsp";
		} else if(cmd.equals("login")) {
			viewpage = "/WEB-INF/views/login/login.jsp";
		} else {
			viewpage = "/error/error.jsp";
		}
		
		// view 지정
		RequestDispatcher dis = request.getRequestDispatcher(viewpage);
		
		// forward
		dis.forward(request, response);
	}


	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doProcess(request, response, "GET");
	}


	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doProcess(request, response, "POST");
	}
}
```
`urlPatterns = { "/Board" }`
- `@WebServlet` 어노테이션을 사용하여 서블릿을 매핑
- url에 `/Board가 입력되면 이 서블릿이 호출됨
  
`doProcess`
- 요청을 처리하는 메소드
- 현재 get 방식, post 방식 모두 이 메소드로 요청을 처리

`String cmd = request.getParameter("cmd")`
- 요청 파라미터 중 "cmd"의 값을 가져옴
- 이 값을 기반으로 요청을 판단하여 적절한 작업을 수행합니다.

`RequestDispatcher dis = request.getRequestDispatcher(viewpage)`
- 요청을 처리한 후에 보여줄 뷰 페이지를 설정하기 위해 `RequestDispatcher`를 가져옴

`dis.forward(request, response)`
- `RequestDispatcher`를 사용하여 요청을 해당 뷰 페이지로 포워딩