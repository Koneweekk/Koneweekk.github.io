---
title: Vue - 라우터
date: 2024-06-05 14:50:00 +09:00
categories: [Front-End, Vue]
tags: [Front-End, Javascript, Vue, Router]
---

## Ⅰ. Routing

### 1. Routing이란?

- 네트워크에서 경로를 선택하는 프로세스
- 웹 서비스에서의 라우팅 : 유저가 방문한 URL에 대해 적절한 결과를 응답

---

### 2. Routing in SSR
- Server가 모든 라우팅을 통제
- URL로 요청이 들어오면 응답으로 완성된 HTML 제공
- 결론적으로, Routing(URL)에 대한 결정권을 서버가 가짐
  
---

### 3. Routing in SPA / CSR
- 서버는 하나의 HTML(index.html)만을 제공
- 이후에 모든 동작은 하나의 HTML 문서 위에서 JavaScript 코드를 활용
- 즉, 하나의 URL만 가질 수 있음

---

### 4. Routing이 없다면?
- 유저가 URL을 통한 페이지의 변화 감지 불가능
- 페이지가 무엇을 렌더링 중인지에 대한 상태를 알 수 없음
  - 새로고침 시 처음 페이지로 돌아감
  - 링크 공유 시 처음 페이지만 공유 가능
- 브라우저의 뒤로 가기 기능 사용 불가능

---
<br>

## Ⅱ. Vue Router

### 1. Vue Router란?

> Vue의 공식 라우터
> 
SPA 상에서 라우팅을 쉽게 개발할 수 있는 기능을 제공
- 라우트에서 컴포넌트를 매핑한 후, 어떤 URL에서 렌더링할지 알려줌
- 즉, SPA를 MPA처럼 URL을 이동하면서 사용 가능
- URL이 변경되지 않는다는 SPA 단점을 해결

---

### 2. Vue Start Setting
  
패키지 매니저를 이용하여 뷰 라이터 설치

```
npm install vue-router@4
```

---

### 3. router-link와 router-view

App.vue에 `router-link` 요소 및 `router-view`가 추가

```html
<!-- App.vue -->
<template>
  <div id="app">
    <nav>
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </nav>
    <router-view/>
  </div>
</template>
```

#### router-link
- a 태그와 비슷한 기능 -> URL을 이동시킴
  - routes에 등록된 컴포넌트와 매핑됨
  - 히스토리 모드에서 router-link는 클릭 이벤트를 차단
- 목표 경로는 `to` 속성으로 지정됨
- 기능에 맞게 HTML에서 a 태그로 렌더링되고, 다른 태그로 변경 가능

#### router-view
- 주어진 URL에 대해 일치하는 컴포넌트를 렌더링하는 컴포넌트
- 실제 component가 DOM에 부착되어 보이는 자리를 의미
- router-link를 클릭하면 routes에 매핑된 컴포넌트를 렌더링

---

### 4. router/index.js 생성

```javascript
const routes = [
  {
    path: '/',
    name: 'home',
    component: HomeView
  },
  {
    path: '/about',
    name: 'about',
    component: () => import('../views/AboutView.vue')
  }
]

const router = new VueRouter({
  mode: 'history',
  base: process.env.BASE_URL,
  routes
})

export default router
```

- 라우터에 관련된 정보 및 설정이 작성된 곳
- routes에 URL와 컴포넌트를 매핑


#### History mode
- 새로고침 없이 URL 이동 기록을 남길 수 있음
- 우리에게 익숙한 URL 구조로 사용 가능
- Default값은 Hash mode(`#`을 통해 URL을 구분하는 방식)

#### ※ lazy - loading

- 모든 파일을 한 번에 로드하면 시간이 매우 오래 걸림
- 특정 라우트에 방문할 때만 매핑 컴포넌트 로드
  - 최초 로드하는 시간이 빨라짐

```javascript
// router/index.js
  {
    path: '/about',
    name: 'about',
    component: () => import( '../views/AboutView.vue')
  },
```

### 5. views와 components

#### views
- `router-view`에 들어갈 컴포넌트 작성
- routes에 매핑되는 컴포넌트
- 다른 컴포넌트와 구분하기 위해 View로 끝나도록 이름을 만듦

#### components
- routes에 매핑된 컴포넌트의 하위 컴포넌트를 모아두는 폴더

---
<br>

## Ⅲ. Vue Router 활용

### 1. 선언적 방식 네비게이션
- `router-link`의 `to`속성으로 주소 전달
- `routes`에 등록된 주소와 매핑된 컴포넌트로 이동
- 동적인 값을 사용게되면 v-bind를 사용해야 정상적으로 작동
  
```html
<router-link to="/">Home</router-link>
<router-link :to="{name: 'home'}">Home</router-link>
```

---

### 2. 프로그래밍 방식 네비게이션

- Vue 인스턴스 내부에서 라우터 인스턴스에 `$router`로 접근 가능
- 다른 URL로 이동하려면 `this.$router.push`를 사용
  - history stack에 이동할 URL을 넣는 방식
  - history stack에 기록이 남아 브라우저의 뒤로 가기 버튼 사용 가능

```html
<!-- AboutView.vue -->
<template>
  <div class="about">
    <h1>This is an about page</h1>
    <button @click='toHome'>홈으로</button>
  </div>
</template>
<script>
export default {
  name: 'AboutView',
  methods: {
    toHome() {
      this.$router.push({name:'home'})
    }
  }
}
</script>
```

---
<br>

## Ⅳ. 동적 라우팅

### 1. 동적 라우팅 설정

- URL의 특정 값을 변수처럼 사용 가능
- route를 추가할 때 동적 인자를 `path`에서 `:`를 이용하여 명시
  
``` javascript
// router/index.js
import HelloView from '../views/HelloView.vue'

...
const routes = [
  {
    path: '/hello/:userName',
    name: 'hello',
    component: HelloView
  }
]
...
```

- `$route.params`로 변수에 접근 가능
- data에 넣어서 사용하는 것을 권장

```html
<!-- HelloView.vue -->
<template>
  <div>
    <h1>hello, {{ $route.params.userName }}</h1>
    <h1>hello, {{ userName }}</h1>
  </div>
</template>

<script>
export default {
  name: 'HellowView',
  data() {
    return {
      userName: this.$route.params.userName
    }
  }
}
</script>
```

Vue3의 CompositionAPI로 코드를 바꾸면 다음과 같다

```html
<script setup>
import { ref } from 'vue';
import { useRoute } from 'vue-router';

const route = useRoute();
const userName = ref(route.params.userName);
</script>
```

---

### 2. 선언적 방식 네비게이션

- params를 이용하여 동적 인자 전달 가능
  
```html
<!-- App.vue -->
<router-link :to="{name: 'hello', params: { userName: 'harry'}}">Hello</router-link>
```

---

### 3. 프로그래밍 방식 네비게이션

- 직접 데이터를 입력받아 router를 통해 전달
  
```html
<template>
  <div class="about">
    <h1>This is an about page</h1>
    <button @click='toHome'>홈으로</button>
    <input type="text" v-model="inputData" @keyup.enter="goToHello">
  </div>
</template>
<script>
export default {
  name: 'AboutView',
  data() {
    return{
      inputData: null
    }
  },
  methods: {
    toHome() {
      this.$router.push({name:'home'})
    },
    goToHello() {
      this.$router.push({name:'hello', params: {userName: this.inputData}})
    }
  }
}
</script>
```

VCompositionAPI로 코드를 바꾸면 다음과 같다

```html
<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();
const inputData = ref(null);

const toHome = () => {
  router.push({ name: 'home' });
};

const goToHello = () => {
  router.push({ name: 'hello', params: { userName: inputData.value } });
};
</script>
```

---
<br>

## Ⅴ. 네비게이션 가드

### 1. 네비게이션 가드란?

> Vue router로 URL에 접근할 때 로직에 따라 다른 url로 redirect를 하거나 해당 URL로의 접근을 막는 방법

#### 네비게이션 가드의 종류

전역 가드
- 애플리케이션 전역에서 동작
  
라우터 가드
- 특정 URL에서만 동작
  
컴포넌트 가드
- 라우터 컴포넌트 안에 정의

---

### 2. 전역 가드

- 다른 url 주소로 이동할 때 항상 실행
- `router/index.js에 router.beforeEach()`를 사용하여 설정
- URL이 변경되어 화면이 전환되기 전 `router.beforeEach()`가 호출됨
  - 화면이 전환되지 않고 대기 상태가 됨
- 변경된 URL로 라우팅하기 위해서는 `next()`를 호출해줘야함
  - `next()`가 호출되기 전까지 화면이 전환되지 않음

```javascript
// router/index.js
router.beforeEach((to, from, next) => {
  // 로그인 여부
  const isLoggedIN = false
  // 로그인이 필요한 페이지
  const authPages = ['hello']
  // 앞으로 이동할 페이지가 로그인이 필요한 사이트인지 확인
  const isAuthRequired = authPages.includes(to.name)
  
  // 로그인 여부 판단에 따른 페이지 이동
  if (! isAuthRequired || isLoggedIN) {
    console.log('to로 이동')
    next()
  } else {
    console.log('login으로 이동')
    next({name:'login'})
  }
})
```

- `to` : 이동할 URL 정보가 담긴 Route 객체
- `from` : 현재 URL 정보가 담긴 Route 객체
- `next` : 지정한 URL로 이동하기 위해 호출하는 함수
  - 콜백 함수 내부에서 반드시 한 번만 호출되어야 함
  - 기본적으로 `to`에 해당하는 URL로 이동

---

### 3. 라우터 가드

> 전체 route가 아닌 특정 route에 대해서만 가드를 설정하고 싶을 때 사용

```js
// router/index.js
const isLoggedIn = true

const routes = [
  {
    path: '/login',
    name: 'login',
    component: LoginView,
    beforeEnter(to, from, next) {
      if (isLoggedIn === true) {
        console.log('이미 로그인 함')
        next({name: 'home'})
      } else {
        next()
      }
    }
  }
]
```

`beforEnter()`
- route에 진입했을 때 실행
- 라우터를 등록한 위치가 추가
- 단 매개변수, 쿼리, 해시 값이 변경될 때는 실행되지 않고 다른 경로에서 탐색할 때만 실행됨
- 콜백 함수는 `to`, `from`, `next`를 인자로 받음

---

### 4. 컴포넌트 가드

> 특정 컴포넌트 내에서 가드를 지정하고 싶을 때 사용
  
```js
// view/HelloView.vue
<script>
export default {
    name: 'HelloView',
    data() {
        return {
            userName: this.$route.params.userName
        }
    },
    beforeRouteUpdate(to, from, next) {
        this.userName = to.params.userName
        next()
    }
}
</script>
```

`beforeRouteUpdate()`
- 해당 컴포넌트를 렌더링하는 경로가 변경될 때 싱행

---

### 5. 404 Not Found

> 요쳥한 리소스가 존재하지 않는 경우

- 모든 경로에 대해서 404 page로 redirect 시키기
- 기존에 명시한 경로가 아닌 모든 경로가 404 page로 redirect됨
- 이 때, `routes`에 최하단부에 작성해야 함

```js
const routes = [
  ...
  {
    path: '/404',
    name: 'NotFound404',
    component: NotFound404
  },
  {
    path: '*',
    redirect: '/404'
  }
]
```