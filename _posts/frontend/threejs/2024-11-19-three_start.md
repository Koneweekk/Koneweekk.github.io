---
title: Three.js - 설치 및 시작하기
date: 2024-11-19 12:50:00 +09:00
categories: [Front-End, Three.js]
tags: [Front-End, Javascript, Three.js]
---

## Ⅰ. 설치하기

### 1. npm

three.js npm을 포함한 빌드 툴에서 설치가 가능하다.
- 이 경우 모든 패키지를 한 파일에 결합시켜주는 번들링 툴(webpack, vite 등)을 사용하게 될 것이다.

```bash
npm install three
```

위 코드를 통해 패키지가 다운로드 되면 다음과 같이 코드에서 사용할 수 있다.

```js
import * as THREE from 'three';
```

---

### 2. 빌드 툴 없이 사용하기

별다른 빌드 시스템없이 CDN을 통해 사용할 수 있다.

```html
<script type="importmap">
    {
        "imports": {
            "three": "https://cdn.jsdelivr.net/npm/three@<version>/build/three.module.js",
            "three/addons/": "https://cdn.jsdelivr.net/npm/three@<version>/examples/jsm/"
        }
    }
</script>
```

이후 npm 방식과 똑같이 three를 import 할 수 있다.

```html
<script type="module">
    import * as THREE from 'three';
</script>
``` 

여기서 `three/addons/`는 추가적인 모듈을 사용할 때 필요하다.
- 컨트롤러(`controls`)나 외부 모델 로더(`GLTFLoader`)등을 불러올 때 사용된다.

```js
import * as THREE from 'three';
import { OrbitControls } from 'three/addons/controls/OrbitControls.js';
import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js';

const controls = new OrbitControls( camera, renderer.domElement );
const loader = new GLTFLoader();
```

---
<br>

## Ⅱ. 주요 구성 요소

![01](/assets/img/post/frontend/three/2024-11-19-three_start/01.png)

### 1. Scene

모든 것은 씬(scene)에 포함된다.
- 배경색(background color), 안개(fog) 등의 요소를 포함
- 카메라, 라이트, 오브젝트를 포함

```js
const scene = new THREE.Scene();
```

---

### 2. Camera

카메라는 씬을 바라보는 시점을 결정한다. 카메라의 종류는 다음과 같다.
  - `PerspectiveCamera` : 원근 카메라
  - `OrthographicCamera` : 직교 카메라  

```js
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 100);
```

---

### 3. Renderer

Renderer는 씬을 그리는 역할을 한다.
- 즉, 씬을 화면에 렌더링하는 역할

```js
const renderer = new THREE.WebGLRenderer();
``` 

---

### 4. Mesh

Mesh는 3D 공간에 배치되는 기본 객체이다.
- 모양(geometry)과 재질(material)을 가진다.
- 카메라와 라이트에 의해 비춰지고 그리기 위해 렌더러에 의해 처리된다.

```js
const mesh = new THREE.Mesh( geometry, material );
```

--- 

### 5. Material

Material은 메시의 재질을 결정한다.
- 색상, 반사율, 투명도 등의 속성을 가진다.

```js
// 녹색 재질
const material = new THREE.MeshBasicMaterial( { color: 0x00ff00 } );
```

---

### 6. Geometry

Geometry는 메시의 모양을 결정한다.
- 오브젝트의 형태를 정의하는 역할

```js
// 박스 모양의 기하학적 객체
const geometry = new THREE.BoxGeometry(1, 1, 1);
```

---

### 7. 예제 코드

```js
import * as THREE from "three";

// 장면
const scene = new THREE.Scene();
scene.background = new THREE.Color(0xffffff);

// 카메라
const camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 0.1, 1000);

// 렌더러
const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
renderer.setSize(window.innerWidth, window.innerHeight);
renderer.setAnimationLoop(animate);
$('body').append(renderer.domElement);

// 오브젝트
const geometry = new THREE.BoxGeometry(0.5, 0.5, 0.5);
const material = new THREE.MeshBasicMaterial({ color: 0x4fd1c5 });
const cube = new THREE.Mesh(geometry, material);
const edges = new THREE.EdgesGeometry(geometry);
const line = new THREE.LineSegments(
  edges,
  new THREE.LineBasicMaterial({ color: 0x000000 })
);
cube.add(line);
scene.add(cube);

// 카메라 위치
camera.position.z = 3;

// 애니메이션
function animate() {
  cube.rotation.x += 0.01;
  cube.rotation.y += 0.01;

  renderer.render(scene, camera);
}
```

다음과 같은 코드를 통해 회전하는 정육면체를 렌더링할 수 있다.

![02](/assets/img/post/frontend/three/2024-11-19-three_start/02.png)
