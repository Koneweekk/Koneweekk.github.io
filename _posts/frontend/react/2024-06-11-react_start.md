---
title: React - 리액트 시작하기
date: 2024-06-11 09:30:00 +09:00
categories: [Front-End, React]
tags: [Front-End, Javascript, React]
---

## Ⅰ. React란?

> 2014년 페이스북에서 사용자 인터페이스(User Interface)를 만들기 위해 개발된 
Javascript 기반의 라이브러리 프로그램

### 1. React를 사용하는 이유

- UI를 자동으로 업데이트
- Javascript의 문법을 그대로 활용
- 페이스북의 지속적인 관리와 함께 커뮤니티가 활성화
- 리액트 기반의 React Native라는 기술을 통해 iOS, 안드로이드 기반의 모바일 애플리케이션도 개발 가능

---

### 2. React 시작하기

- CLI를 통해 react 프로젝트 생성
```
npx create-react-app react-practice
```

- react 프로젝트 폴더로 이동 후 패키지 다운로드
```
cd react-practice

npm install
```


- react 프로젝트 시작
```
npm start
```


---
<br>

## Ⅱ. React 특징

### 1. 선언적 프로그래밍

> 무엇을 해결할것인지에 대해 중점적으로 맞춰져있는 프로그래밍

- 원하는 결과를 View 에만 초점을 두고 우리가 원하는 모습을 리액트에게 전달
- “어떻게" 하는지에 대한 중간과정은 리액트가 알아서 처리
- 개발 과정에서 최종 결과물의 모습만 고려하면 되기 때문에 훨씬 편리하고 효율적으로 개발 가능
  
### 2. Virtual DOM

- React에서 UI를 업데이트하고자 할 때 내부적으로 가상 DOM을 이용
- DOM 요소에 변화를 주기 전 실제 DOM에 일어나야 하는 변화를 계산해서 보여주는 기능을 수행


### 3. Component

> UI적인 면에서 재활용이 가능한 UI 구성단위

- 필요한곳에서 재사용 할수 있음
- 독립적으로 사용할 수 있기 때문에 코드 유지보수가 쉬움
- 다른 컴포넌트를 포함시킬수 있음
- 해당 페이지가 어떻게 구성되어 있는지 한눈에 파악할수 있음

#### 클래스 컴포넌트

```js
import React from 'react';

class App extends React.Component {
  render() {
    return <h1>This is Class Component!</h1>;
  }
}

export default App;
```

- 반드시 `render()` 메서드가 있어야 함
- 그 안에서 화면에 보여줄 JSX(Javascript Syntax eXtension)를 반환
- state 및 lifecycle(라이프사이클) API를 통해 관련 기능을 사용 가능

#### 함수 컴포넌트
import React from 'react'

const App = () => {
  return <h1>This is Function Component!</h1>;
};

- render 메서드 없이 JSX를 반환하는 방식으로, 클래스 컴포넌트에 비해서 훨씬 간단하고 단순
- React 16.8 버전에서 Hook 기능이 추가되면서 함수 컴포넌트에서 Hook 구현 가능
- 함수 컴포넌트가 구현할수 있는 state이외 많은 기능들을 사용할수 있어서 편리하다는 큰 장점 보유

### 4. JSX(Javascript Syntax eXtension)

> Javascript Syatax eXtension 문법 종류

- 자바스크립트 파일 어디에서나 필요한 곳에 HTML처럼 작성 가능
- 변수에 저장할 수도 있고, 함수의 인자로 넘길 수도 있다.

---
<br>

## Ⅲ. JSX(Javascript Syntax eXtension)

### 1. JSX란?

- 자바스크립트 파일 어디에서나 필요한 곳에 HTML처럼 작성 가능
- 변수에 저장할 수도 있고, 함수의 인자로 넘길 수도 있다.

```js
const Login = () => {
	const [id, setID] = useState("");
	const saveUserID = (event) => {
		setID(event.target.value);
	};
    
    return(
      <div>
          <input
            className="input_e-mail"
            type="text"
            placeholder="이메일"
            value={id}
            onChange={saveUserID}
          ></input>
      </div>
	);
}
```

---

### 2. JSX 표현식

- `{ }` 안에 유효한 자바스크립트 표현식을 작성할 수 있음

```js
import React from 'react'

const Greetings = () => {
  const name = 'Koneweekk';

  return (
  	<h1>{name}님, Welcome to Koenweekk!</h1>
  );
};

export default Greetings;
```

---

### 3. Event 처리

JSX에선 직접 이벤트와 이벤트 핸들러(Event Handler)를 부여할 수 있는 방식으로 처리
- 이벤트는 앞에 on을 붙여 camelCase로 작성.
- 문자열이 아닌 함수로 이벤트 핸들러를 전달

```js
<h1 className="title" onClick={handleClick}>Welcome to Koenweekk!</h1>
```

---

### 4. Style 처리

style 속성은 camelCase를 요소로 가지는 자바스크립트 객체를 받는다
- JSX에서 inline 태그를 이용해서 styling을 할 경우, 중괄호를 두 번 겹쳐서 쓰는 형태로 사용

```js
{% raw %}
<h1 style={{ color: "red", backgroundImage: "yellow" }}>
  Welcome to Koneweekk!
</h1>{% endraw %}
```

---

### 5. Self-Closing Tag

JSX 문법에선 어떤 태그라도 self closing tag를 사용할수 있음

```html
<div className="login">
  <div className="logo">
    <img className="logo1" src={logo1} alt="로고1"></img>
  </div>
</div>
```

---

### 6. Nested JSX

JSX 에서 HTML 태그를 사용할땐 반드시 최상위를 감싸고 있는 하나의 태그가 있어야 한다.

```js
// 에러 발생
const Greetings = () => {
	return (
		<h1>Koneweekk님!</h1>
	);
}
```

```js
// 정상 동작
const Greetings = () => {
  return (
    <div>
      <h1>Koneweekk님!</h1>
    </div>
  );
}
```

---

### 7. React.Fragment

최상위 태그가 불필요할 때 사용하는 것이 `<React.Fragment>` 태그
- 추가 DOM element를 생성하지 않고 하나의 컴포넌트 안에 여러 요소(자식)를 간단하게 그룹화할 수 있는 기능
- 또한 축약형 `<></>` 으로도 Fragment를 구현할수 있다.

```js
// React.Fragment 사용
const Greetings = () => {
  return (
    <React.Fragment>
      <h1>Koneweekk님!</h1>
    </React.Fragment>
  );
}
```

```js
// 축약형 Fragment 사용
const Greetings = () => {
  return (
    <>
      <h1>Koneweekk님!</h1>
    </>
  );
}
```