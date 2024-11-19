---
title: Three.js - Mesh 기본
date: 2024-11-19 14:10:00 +09:00
categories: [Front-End, Three.js]
tags: [Front-End, Javascript, Three.js]
---


## Ⅰ. 도형의 생성

### 1. Geometry

Geometry는 3D 객체의 모양을 정의하는 객체이다.
- three.js에서는 기본적으로 여러 종류의 Geometry를 제공한다.

```js
const geometry = new THREE.BoxGeometry(0.5, 0.5, 0.5);
```

위 코드는 박스 모양의 기하학적 객체를 생성한다.

---

### 2. Material

Material은 메시의 재질을 결정한다.
- 색상, 반사율, 투명도 등의 속성을 가진다.

```js
const material = new THREE.MeshBasicMaterial({color:0x4fd1c5 });
```

---

### 3. Mesh

Mesh는 선언해둔 Geometry와 Material을 결합하여 3D 객체를 생성한다.

```js
const cube = new THREE.Mesh(geometry, material);
```

---

### 4. 모서리 표시

모서리를 표시하기 위해서는 `EdgesGeometry`와 `LineBasicMaterial`을 사용한다.

```js
const edges = new THREE.EdgesGeometry(geometry01);
const line = new THREE.LineSegments(
  edges,
  new THREE.LineBasicMaterial({ color: 0x000000 })
);

cube01.add(line);
```

--- 

### 5. 도형의 위치

도형의 위치는 `position` 속성을 사용하여 설정할 수 있다.

```js
cube.position.x = 1;
```

---
<br>

## Ⅱ. 도형의 재질


### 1. 도형의 재질이란?

> 도형의 색상, 반사율, 투명도 등의 속성을 의미

- three.js에서는 여러 종류의 Material이란 재질 객체를 제공한다.
- 이 때 빛의 반사 특성을 고려하는 재질과 그렇지 않은 재질이 있다.

```js
const pointLight = new THREE.PointLight(0xffffff, 500);
pointLight.position.set(0, 2, 12);
scene.add(pointLight);
```

위와 같은 광원에 대한 코드를 추가해야 빛의 반사 특성을 고려한 재질의 특징이 드러난다.

---

### 1. MeshBasicMaterial

단순한 색상만을 가지는 재질이다.

```js
const material = new THREE.MeshBasicMaterial({color:0x4fd1c5 });
```

![01](/assets/img/post/frontend/three/2024-11-19-three_mesh/01.png)

---

### 2. MeshStandardMaterial

빛의 반사 특성을 고려한 재질이다.

```js
const material = new THREE.MeshStandardMaterial({color:0x4fd1c5 });
```

![02](/assets/img/post/frontend/three/2024-11-19-three_mesh/02.png)

빛의 반사를 고려한 재질은 빛의 방향에 따라 색상이 달라진다.

---

### 3. MeshLambertMaterial

이 재질은 다음과 같은 특성을 지닌다.
- 빛의 확산 반사(diffuse reflection)만 계산
- 정반사(specular reflection)는 표현하지 않음
- 광원의 위치에 따라 음영이 생김
- 계산이 비교적 단순해서 성능이 좋음

```js
const material = new THREE.MeshLambertMaterial({color:0x4fd1c5 });
```

![03](/assets/img/post/frontend/three/2024-11-19-three_mesh/03.png)

---

### 4. MeshMatcapMaterial

사전 렌더링된 재질 캡처를 사용하여 조명 효과를 시뮬레이션하는 재질
- 실시간 조명 계산 없이도 복잡한 조명 효과를 표현할 수 있어 성능이 매우 좋다.
- 동적 조명에 반응하지 않음
- 뷰어의 시점에 따라 재질 모습이 변함
- 실제 3D 공간의 조명을 반영하지 않음

```js
const material = new THREE.MeshMatcapMaterial({color:0x4fd1c5 });
```

![04](/assets/img/post/frontend/three/2024-11-19-three_mesh/04.png)

---

### 5. MeshNormalMaterial

3D 객체의 법선 벡터를 RGB 색상으로 시각화하는 재질
- 주로 디버깅이나 특수 효과에 사용된다.
- 조명 계산이 필요 없음
- 모델의 형태를 직관적으로 파악 가능
-실제 재질로 사용하기에는 제한적
-조명에 반응하지 않음

```js
const material = new THREE.MeshNormalMaterial();
```

![05](/assets/img/post/frontend/three/2024-11-19-three_mesh/05.png)

---

### 6. MeshPhongMaterial

광택이 있는 표면을 표현할 수 있는 재질
- 확산 반사(diffuse)와 정반사(specular)를 모두 계산
- 플라스틱이나 금속 같은 광택 있는 표면을 표현하는데 적합
- 성능과 품질의 좋은 균형
- MeshLambertMaterial보다 성능 부하가 큼
- MeshStandardMaterial보다 덜 사실적

```js
const material = new THREE.MeshPhongMaterial({color:0x4fd1c5 });
```

![06](/assets/img/post/frontend/three/2024-11-19-three_mesh/06.png)

---

### 7. MeshToonMaterial

카툰 렌더링을 구현하는 재질
- 애니메이션이나 만화같은 스타일의 비사실적 렌더링을 만들 때 사용
- 독특한 카툰/애니메이션 스타일 구현 가능
- 성능이 비교적 좋음
- 스타일리시한 비사실적 렌더링
- 사실적인 재질 표현 불가
- 그라데이션 맵 없이는 효과가 제한적
- 복잡한 조명 효과 표현 어려움

```js
const material = new THREE.MeshToonMaterial({color:0x4fd1c5 });
```

![07](/assets/img/post/frontend/three/2024-11-19-three_mesh/07.png)

---

### 8. MeshPhysicalMaterial

물리적 기반의 재질
- MeshStandardMaterial을 확장한 재질
- 더 많은 물리 기반 속성을 제공하여 더욱 사실적인 재질 표현이 가능
- 다양한 물리적 속성 제어 가능
- 가장 높은 성능 부하
- 설정해야 할 파라미터가 많음

```js
const material = new THREE.MeshPhysicalMaterial({color:0x4fd1c5 });
```

![08](/assets/img/post/frontend/three/2024-11-19-three_mesh/08.png)

다양한 파라미터를 조절하여 자동차 페인트 재질을 표현하였다.

---

### ※ 주요 파라미터

기본 색상
- `color: 0xff0000`

거칠기 (0: 매끄러움, 1: 거침)
- `roughness: 0.5`

금속성 (0: 비금속, 1: 금속)
- `metalness: 0.5`

투명도 사용 여부
- `transparent: false`

불투명도
- `opacity: 1.0`

와이어프레임
- `wireframe: false`

---
<br>

## Ⅲ. 텍스쳐 맵핑

> 텍스쳐 맵핑이란 3D 객체에 이미지를 적용하는 기법

### 1. 텍스쳐 로더 

텍스쳐 로더란 이미지 파일을 로드하는 객체이다.
- `load()` 메서드를 사용하여 이미지 파일을 로드한다.

```js
const textureLoader = new THREE.TextureLoader();
const textureBaseColor = textureLoader.load("./texture/Stylized_Ice_001_basecolor.png");
```

---

### 2. 텍스쳐 맵핑 적용

텍스쳐 맵핑을 위해서는 `map` 속성을 사용한다.
- 이 때 `color` 속성을 사용하지 않는다.

```js
const material = new THREE.MeshStandardMaterial({ map: textureBaseColor });
```

![09](/assets/img/post/frontend/three/2024-11-19-three_mesh/09.png)

---

### 3. noramlMap

법선 벡터를 사용하여 표면의 거칠기를 시뮬레이션하는 텍스쳐

```js
const textureNormal = textureLoader.load("./texture/Stylized_Ice_001_normal.png");

const material = new THREE.MeshStandardMaterial({ 
  map: textureBaseColor,
  normalMap: textureNormal
});
```

![10](/assets/img/post/frontend/three/2024-11-19-three_mesh/10.png)


--- 

### 4. displacementMap

표면의 높이를 시뮬레이션하는 텍스쳐
- `displacementScale` 파라미터를 사용하여 높이를 조절할 수 있다.

```js
const textureDisplacement = textureLoader.load("./texture/Stylized_Ice_001_displacement.png");

const material = new THREE.MeshStandardMaterial({ 
  map: textureBaseColor,
  normalMap: textureNormal,
  displacementMap: textureDisplacement,
  displacementScale: 0.02
});
```

![11](/assets/img/post/frontend/three/2024-11-19-three_mesh/11.png)

---

### 5. roughnessMap

표면의 거칠기를 시뮬레이션하는 텍스쳐
- roughness 파라미터를 사용하여 거칠기를 조절할 수 있다.

```js
const textureRoughness = textureLoader.load("./texture/Stylized_Ice_001_roughness.png");

const material = new THREE.MeshStandardMaterial({ 
  map: textureBaseColor,
  normalMap: textureNormal,
  displacementMap: textureDisplacement,
  displacementScale: 0.03,
  roughnessMap: textureRoughness,
  roughness: 0.8
});
```

![12](/assets/img/post/frontend/three/2024-11-19-three_mesh/12.png)
