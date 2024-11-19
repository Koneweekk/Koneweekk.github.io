---
title: Three.js - Camera와 빛
date: 2024-11-19 15:35:00 +09:00
categories: [Front-End, Three.js]
tags: [Front-End, Javascript, Three.js]
---

## Ⅰ. Camera

### 1. three 카메라 객체의 속성

three.js에서 제공하는 카메라 객체에서 주로 사용하는 객체는 아래와 같다.
```js
const fov = 63;  // 화각
const aspect = window.innerWidth / window.innerHeight;  // 종횡비
const near = 0.1;  // 카메라 시점이 시작되는 위치
const far = 1000;  // 카메라 시점이 끝나는 위치

const camera = new THREE.PerspectiveCamera( fov, aspect, near, far );
```

---

### 2. 화각(Field of View)

> 화각이란 카메라가 촬영하는 영역의 각도를 의미

화각이 커질수록 촬영 영역이 넓어지고, 화각이 작아질수록 촬영 영역이 좁아진다.
- 확대 촬영하고 싶다면 화각을 줄이고, 넓은 영역을 촬영하고 싶다면 화각을 늘린다.
    - 망원의 화각 : 28˚ 이하
    - 광각의 화각 : 63˚ 이상
- 이 수치는 three.js에서 제공하는 `PerspectiveCamera`의 `fov` 속성을 통해 조절할 수 있다.

<img src="/assets/img/post/frontend/three/2024-11-19-three_camera/01.png" width="50%">
- 28도의 화각

<img src="/assets/img/post/frontend/three/2024-11-19-three_camera/02.png" width="50%">
- 63도의 화각

---

### 3. 종횡비(Aspect Ratio)

> 종횡비란 카메라가 촬영하는 영역의 가로와 세로의 비율을 의미

종횡비는 카메라가 촬영하는 영역의 가로와 세로의 비율을 의미한다.
- 종횡비가 1일 경우 정사각형을, 종횡비가 2일 경우 가로가 세로의 두 배인 영역을 촬영한다.
- 이 수치는 three.js에서 제공하는 `PerspectiveCamera`의 `aspect` 속성을 통해 조절할 수 있다.
- 보통 three.js에선 브라우저의 뷰포트를 기준으로 카메라를 설정한다.

---

### 4. 카메라 시점이 시작되는 위치와 끝나는 위치

![03](/assets/img/post/frontend/three/2024-11-19-three_camera/03.png)

near보다 가까이 있거나, far보다 멀리 있는 물체는 카메라에 반영되지 않는다.
- 즉, 이 정해둔 거리를 벗어나는 객체는 랜더링하지 않는다.
- 이 수치는 three.js에서 제공하는 `PerspectiveCamera`의 `near`와 `far` 속성을 통해 조절할 수 있다.

![04](/assets/img/post/frontend/three/2024-11-19-three_camera/04.png)

위 그림은 `near` 속성을 증가시켜 가까이 있는 부분의 렌더링을 막은 것이다.

---

### 5. 카메라의 위치 조정

크게 두 가지 방법이 존재한다.

- 첫 번째 방법은 `camera.position.set()` 메서드를 사용하는 방법이다.

```js
camera.position.set( 0, 0, 10 );
```

- 두 번째 방법은 `camera.position.x`, `camera.position.y`, `camera.position.z` 속성을 직접 조절하는 방법이다.

```js
camera.position.x = 10;
camera.position.y = 10;
camera.position.z = 10;
```

여기서 각 축이 의미하는 바는 아래와 같다.
- x : 왼쪽 오른쪽
- y : 위 아래
- z : 앞 뒤

추가적으로 `camera.lookAt()` 메서드를 사용하면 카메라의 시점을 조절할 수 있다.
- 기본적으로 camera의 시점은 (0, 0, 0) 좌표를 향하고 있음

```js
camera.lookAt( new THREE.Vector3( 2, 3, 4 ) );
```

---
<br>

## Ⅱ. 빛

### 1. AmbientLight

전역 조명으로, 장면 전체를 균일하게 비추는 빛
- 실제 세계의 간접 조명을 시뮬레이션하는데 사용
- 파라미터는 각각 색상과 강도를 의미

```js
const ambientLight = new THREE.AmbientLight( 0xffffff, 0.5 );
```

조명강도 0.5와 2의 차이는 다음과 같다.

![05](/assets/img/post/frontend/three/2024-11-19-three_camera/05.png)

![06](/assets/img/post/frontend/three/2024-11-19-three_camera/06.png)

---

### 2. DirectionalLight

특정 방향으로 향하는 조명
- 태양광과 같이 멀리 있는 광원을 시뮬레이션
- 모든 광선이 평행하게 진행
- 광원의 위치가 아닌 방향만이 중요

```js
const directionalLight = new THREE.DirectionalLight( 0xffffff, 3 );

scene.add( directionalLight );
```

`DirectionalLightHelper`를 사용하면 빛의 방향을 시각화할 수 있다.

```js
const dlHelper = new THREE.DirectionalLightHelper( directionalLight, 2 );

scene.add( dlHelper );s
```

![07](/assets/img/post/frontend/three/2024-11-19-three_camera/07.png)

---

### 3. HemisphereLight

하늘색과 지면색을 사용하여 위와 아래에서 오는 그러데이션 조명
- 주로 실외 장면의 자연스러운 환경광을 시뮬레이션하는데 사용

```js
const hemisphereLight = new THREE.HemisphereLight(0x0000ff, 0xff0000, 3);

scene.add( hemisphereLight );
```

하늘색은 파란색, 지면색은 빨간색으로 지정한 결과는 다음과 같다.
![08](/assets/img/post/frontend/three/2024-11-19-three_camera/08.png)

---

### 4. PointLight

한 점에서 모든 방향으로 빛을 방출하는 광원
- 전구나 촛불과 같은 점광원을 시뮬레이션하는데 사용

```js
const pointLight = new THREE.PointLight(0xffffff, 3);
pointLight.position.set(2, 1, 1);

scene.add(pointLight);
```

![09](/assets/img/post/frontend/three/2024-11-19-three_camera/09.png)

---

### 5. RectAreaLight

형광등이나 창문에서 들어오는 빛과 같이 사각형 영역에서 방출되는 빛을 시뮬레이션하는 조명
- 주로 실내 장면의 조명을 시뮬레이션하는데 사용

```js
const rectAreaLight = new THREE.RectAreaLight(0xffffff, 3);

rectAreaLight.position.set(0, 1, 1);
rectAreaLight.width = 2;
rectAreaLight.height = 2;

scene.add(rectAreaLight);  
```

![10](/assets/img/post/frontend/three/2024-11-19-three_camera/10.png)

---

### 6. SpotLight

한 점에서 원뿔 모양으로 빛을 방출하는 조명
- 무대 조명이나 손전등과 같은 집중 조명을 시뮬레이션하는데 사용

```js
const spotLight = new THREE.SpotLight(0xffffff, 3);
spotLight.position.set(0, 1, 1);

scene.add(spotLight);
```

![11](/assets/img/post/frontend/three/2024-11-19-three_camera/11.png)
