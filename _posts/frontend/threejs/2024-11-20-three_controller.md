---
title: Three.js - 카메라 컨트롤러와 모델 로딩
date: 2024-11-20 09:20:00 +09:00
categories: [Front-End, Three.js]
tags: [Front-End, Javascript, Three.js]
---

## Ⅰ. 카메라 컨트롤러

### 1. OrbitControls 불러오기

`OrbitControls`를 사용하면 마우스를 드래그하여 카메라를 회전, 확대 및 이동할 수 있다.

`OrbitControls`는 다음과 같은 경로로 불러올 수 있다.

```js
// Three.js 라이브러리와 OrbitControls 모듈 불러오기
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";

// 씬(Scene) 생성
const scene = new THREE.Scene();

// 카메라(Camera) 생성
const camera = new THREE.PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
);
camera.position.set(0, 0, 10);

// 렌더러(Renderer) 생성
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);

// OrbitControls 생성
const controls = new OrbitControls(camera, renderer.domElement);
```

---

### 2. OrbitControls 동작 추가

`OrbitControls` 객체의 `update()` 메서드는 카메라의 위치와 방향을 업데이트한다.

- 그에 따라 사용자 입력에 카메라가 올바르게 동작하도록 한다.
- 이 메서드는 애니메이션 루프 내에서 호출되어야 한다.

```js
// 애니메이션 루프 함수
function animate() {
  requestAnimationFrame(animate);

  // OrbitControls 업데이트
  controls.update();

  // 씬(Scene) 렌더링
  renderer.render(scene, camera);
}

// 애니메이션 루프 시작
animate();
```

---

### 3. 최대/최소 줌 설정

`OrbitControls`를 사용하면 카메라의 최대 및 최소 줌 레벨을 설정할 수 있다.

- 이를 통해 사용자가 카메라를 너무 멀리 또는 너무 가까이 이동하지 않도록 제한할 수 있다.

```js
// 최대 및 최소 줌 레벨 설정
controls.minDistance = 5;
controls.maxDistance = 50;
```

- `minDistance` : 카메라가 이동할 수 있는 최소 거리
- `maxDistance` : 최대 거리를 의미한다.

---

### 4. 최대 조절 각도 설정

`OrbitControls`를 사용하면 카메라의 최대 및 최소 각도를 설정할 수 있다.

- 이를 통해 사용자가 카메라를 특정 각도 이상으로 회전하지 않도록 제한할 수 있다.

```js
// 최대 및 최소 각도 설정
controls.maxPolarAngle = Math.PI / 2;
controls.minPolarAngle = 0;
```

- `maxPolarAngle` : 카메라가 위로 회전할 수 있는 최대 각도 (라디안 단위)
- `minPolarAngle` : 카메라가 아래로 회전할 수 있는 최소 각도 (라디안 단위)

---

<br>

## Ⅱ. RGBLoader

### 1. RGBLoader 불러오기

주로 `.hdr` 형식의 고동적 범위(HDR) 이미지를 로드하는 데 사용된다.

- 이 로더를 사용하면 HDR 이미지를 효율적으로 로드하고 사용할 수 있다.

먼저 `HDRLoader` 모듈을 불러와야 한다. 다음과 같은 경로에서 불러올 수 있다.

```js
import { RGBELoader } from "three/examples/jsm/loaders/RGBELoader.js";
```

#### ※ HDR(High Dynamic Range) 이란?

- 밝은 부분과 어두운 부분의 차이를 효과적으로 보정하여 전반적으로 균형있게 출력할 수 있는 기술

---

### 2. HDR 텍스처 로드

`RGBLoader`를 사용하여 `.hdr` 파일을 로드하는 방법은 다음과 같다.

```js
// RGBLoader 인스턴스 생성
const rgbeLoader = new RGBELoader();

// 텍스처 로드
rgbeLoader.load("img/empty_play_room_4k.hdr", function (texture) {
  texture.mapping = THREE.EquirectangularReflectionMapping;
  scene.background = texture; // 3차원 배경으로 사용
  scene.environment = texture; // 광원으로 사용
});
```

`load`

- 파일을 비동기적으로 로드합니다.
- 파일이 로드되면 콜백 함수가 호출되고, 로드된 텍스처가 `texture` 매개변수로 전달

`EquirectangularReflectionMapping`

- 텍스처가 구형 반사 매핑으로 사용됨을 의미합니다.

`scene.background = texture`

- 장면의 배경으로 HDR 이미지를 사용함을 의미합니다.

`scene.environment = texture`

- 로드된 텍스처를 장면의 환경 맵으로 설정
- 이는 장면의 조명과 반사에 영향

![01](/assets/img/post/frontend/three/2024-11-20-three_controller/01.png)

이미지를 보면 광원의 표현이 제대로 된 것을 확인할 수 있다.

---

### 3. 톤 조정

만약 밝기 톤이 부적절하다 싶으면 톤 조절을 실행시켜줄 수 있다.

```js
renderer.toneMapping = THREE.ACESFilmicToneMapping;
renderer.toneMappingExposure = 0.5;
```

renderer.toneMapping = THREE.ACESFilmicToneMapping;

`ACESFilmicToneMapping` : 고급 톤 매핑 알고리즘
이미지의 밝은 부분과 어두운 부분을 더 자연스럽게 표현합니다.

`toneMappingExposure` : 톤 매핑 노출 값을 설정

- 이 값은 톤 매핑의 강도를 조절하며, 기본값은 1
- 값을 높이면 이미지가 더 밝아지고, 낮추면 더 어두워진다.

![02](/assets/img/post/frontend/three/2024-11-20-three_controller/02.png)

값 조절 결과 반사광이 줄어든 것을 확인할 수 있다.

---

<br>

## Ⅲ. GLTFLoader

### 1. GLTFLoader 불러오기

주로 GLTF (GL Transmission Format) 파일을 로드하는 데 사용된다.

- GLTF는 3D 모델을 효율적으로 전송하고 로드하기 위한 파일 형식
- 웹 애플리케이션에서 3D 모델을 로드하고 렌더링하는데 주로 이용

```js
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader.js";

// GLTFLoader 인스턴스 생성
const gltfLoader = new GLTFLoader();
```

---

### 2. 모델 로드

모델의 로드할 때 사용되는 `load` 메서드의 구성은 다음과 같다.

```js
// 모델로드
gltfLoader.load(
  "img/BoomBox.glb", // 모델 파일 경로
  function (gltf) {
    // 모델 로드 성공 시 호출되는 콜백 함수
    const model = gltf.scene;
    model.castShadow = true;
    model.receiveShadow = true;
    scene.add(model);
  },
  function (xhr) {
    // 모델 로드 진행 상황을 출력하는 콜백 함수
    console.log((xhr.loaded / xhr.total) * 100 + "% loaded");
  },
  function (error) {
    // 모델 로드 실패 시 호출되는 콜백 함수
    console.error(error);
  }
);
```

![03](/assets/img/post/frontend/three/2024-11-20-three_controller/03.png)

성공적으로 모델이 로드된 것을 확인할 수 있다.

---

<br>

## Ⅳ. OBJLoader

### 1. OBJLoader 불러오기

주로 .obj 파일을 로드하고 파싱하는 데 사용된다.

- .obj 파일은 3D 모델링 소프트웨어에서 사용되는 표준 파일 형식 중 하나
- 주로 3D 모델의 기하학적 데이터를 저장하는 데 사용됩니다.
- 텍스처 좌표, 법선 벡터, 그리고 다각형 면을 포함한 정보를 포함
- 텍스처와 재질 정보는 별도의 .mtl 파일에 저장될 수 있다.

```js
import { OBJLoader } from "three/examples/jsm/loaders/OBJLoader.js";

// OBJLoader 인스턴스 생성
const objLoader = new OBJLoader();
```

---

### 2. obj 파일 로드

모델의 로드할 때 사용되는 `load` 메서드의 구성은 다음과 같다.

```js
// 모델 로드
objLoader.load(
    "img/IronMan/IronMan.obj", // 모델 파일 경로
    function (object) {
        // 모델 로드 성공 시 호출되는 콜백 함수
        scene.add(object); // 씬에 모델 추가
    },
    function (xhr) {
        // 모델 로드 진행 상황을 출력하는 콜백 함수
        console.log((xhr.loaded / xhr.total) * 100 + "% loaded");
    },
    function (error) {
        // 모델 로드 실패 시 호출되는 콜백 함수
        console.error(error);
    }
);
```

결과는 다음과 같다.

![04](/assets/img/post/frontend/three/2024-11-20-three_controller/04.png)

---

### 3. mtl 파일

obj 파일은 3D 모델의 기하학적 정보를 포함하고 있지만, 재질 정보는 포함하지 않는다.
- mtl 파일은 obj 파일과 함께 사용되어 모델의 재질 정보를 제공

obj 파일을 제대로 불러오기 위해서는 mtl 파일도 함께 로드해야 한다.
- 이를 위해 MTLLoader를 사용하여 mtl 파일을 먼저 로드한다.
- 이후 OBJLoader를 사용하여 obj 파일을 로드한다.

```js
import { MTLLoader } from "three/addons/loaders/MTLLoader.js";

const mtlLoader = new MTLLoader();

mtlLoader.load('img/IronMan/IronMan.mtl', function (materials) {
    materials.preload();
    objLoader.setMaterials(materials);
    objLoader.load('img/IronMan/IronMan.obj', function (object) {
        scene.add(object);
    }, function (xhr) {
        console.log((xhr.loaded / xhr.total * 100) + '% loaded');
    }, function (error) {
        console.error(error);
    });
});
```

![05](/assets/img/post/frontend/three/2024-11-20-three_controller/05.png)