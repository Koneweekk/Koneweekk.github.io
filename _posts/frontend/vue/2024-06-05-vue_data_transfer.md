---
title: Vue - 컴포넌트간 데이터 통신
date: 2024-06-05 09:40:00 +09:00
categories: [Front-End, Vue]
tags: [Front-End, Javascript, Vue, Props, Emit]
---

## Ⅰ. Pass Props

### 1. Pass Props란?

> 부모 -> 자식으로의 데이터의 흐름

- props는 부모 컴포넌트의 정보를 전달하기 위한 사용자 지정 특성
`prop-data-name="value"`형태로 요소에 속성을 작성하듯이 작성하여 데이터 전달
- 하위 컴포넌트에서도 props에 대해 명시적으로 작성해주어야 함
- 전달받은 props를 type과 함께 명시
- 잘못된 타입이 전달되는 경우 브라우저의 자바스크립트 콘솔에서 사용자에게 경고

```html
<!-- HelloWorld.vue -->
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  }
}
</script>
```

```html
<div id="app">
  <HelloWorld msg="Welcome to Your Vue.js App"/>
</div>
```

---

### 2. Pass Props convention
- 부모에서 넘겨주는 props
  - kebab-case(키값) : HTML 속성명은 대소문자를 구분하지 않음
- 자식에서 받는 props
  - 자식의 스크립트에서 자동으로 camelCae로 변환하여 인식
  - camelCase

---

### 3. Dynamic props

- 변수를 props로 전달할 수 있음
- v-bind directive를 사용해 데이터를 동적을 바인딩
- 부모 컴포넌트의 데이터가 업데이트되면 자식 컴포넌트로 전달되는 데이터 또한 업데이트

```html
<!-- MyComponent.vue -->
<template>
  <div class="border">
    <h1>This is MyComponent</h1>
    <MyChild
    static-props="component에서 child로"
    :dynamic-props="dynamicProps"
    />
  </div>
</template>

<script>
import MyChild from '@/components/MyChild'

export default {
  name: 'MyComponent',
  data: function() {
    return {
      dynamicProps: "It's in data"
    }
  },
  components: {
    MyChild,
  }
}
</script>
```

```html
<!-- MyChild.vue -->
<template>
  <div>
    <h3>This is child component</h3>
    <p>{{staticProps}}</p>
    <p>{{dynamicProps}}</p>
  </div>
</template>

<script>
export default {
  name: 'MyChild',
  props: {
    staticProps: String,
    dynamicProps: String,
  }
}
</script>
```

---
 
### 4. 단방향 데이터 흐름

모든 props는 부모에서 자식으로 즉 아래로 단방향 바인딩을 형성
- 부모 속성이 업데이트되면 자식으로 흐르지만 반대 방향은 아님
- 부모 컴포넌트가 업데이트될 때마다 자식 컴포넌트의 모든 prop들이 최신값으로 새로고침

목적은 다음과 같다.
- 하위 컴포넌트가 상위 컴포넌트 상태를 변경, 데이터 흐름이 복잡해지는 걸 방지
- 하위 컴포넌트에서 prop를 변경하려고 시도해서는 안 됨
- 위의 경우 Vue는 콘솔에서 경고를 출력

---

### 5. Props Composition API

위 예시코드를 다음과 같이 composition API로 바꿀 수 있다.

```html
<!-- MyComponent.vue -->
<template>
  <div class="border">
    <h1>This is MyComponent</h1>
    <MyChild
      :static-props="staticProps"
      :dynamic-props="dynamicProps"
    />
  </div>
</template>

<script setup>
import { ref } from 'vue';
import MyChild from '@/components/MyChild.vue';

const dynamicProps = ref("It's in setup");
const staticProps = "component에서 child로";

</script>
```

```html
<!-- MyChild.vue -->
<template>
  <div>
    <h3>This is child component</h3>
    <p>{{ staticProps }}</p>
    <p>{{ dynamicProps }}</p>
  </div>
</template>

<script setup>
import { defineProps } from 'vue';

const { staticProps, dynamicProps } = defineProps(['staticProps', 'dynamicProps']);

</script>
```

---
<br>

## Ⅱ. Emit Event

### 1.Emit Event란?

> 자식 -> 부모로의 데이터의 흐름

이벤트 발생으로 부모 요소로 데이터를 전달하는 방법
- 데이터를 이벤트 리스너의 콜백함수의 인자로 전달
- 상위 컴포넌트는 해당 이벤트를 통해 데이터를 받음
  
`$emit` 메서드를 통해 부모 컴포넌트에 이벤트 발생
- `$emit('event-name')`형식으로 사용
- 부모 컴포넌트에 event-name이라는 이벤트가 발생했다는 것을 알림

```html
<!-- MyChild.vue -->
<template>
  <div>
    <button @click="ChildToParent">클릭!</button>
  </div>
</template>

<script>
export default {
  ...
  methods: {
    ChildToParent() {
      this.$emit('child-to-parent')
    }
  }
}
</script>
```

```html
<!-- MyComponent.html -->
<template>
  <div class="border">
    <MyChild
    ...
    @child-to-parent="parentGetEvent"
    />
  </div>
</template>

<script>
export default {
  ...
  methods: {
    parentGetEvent() {
      console.log("자식 컴포넌트에서 발생한 이벤트!")
    }
  }
}
</script>
```

`$`
- javascript는 변수에 `_`, `$` 두 개의 특수문자를 사용 가능
- vue는 `$emit`을 이벤트 전달을 위한 방식으로 채택
- 기존에 사용하던 변수, 메서드들과 겹치지 않게 하기 위함

---

### 2. Emit Event Convention

- emit 이벤트를 발생, HTML 요소가 이벤트를 청취 : kebab-case
- 메서드, 변수명 등은 JavaScript에서 사용 : camelCase

---

### 3. Emit Event로 데이터 전달하기

- 이벤트를 발생시킬 때 인자로 데이터를 전달 가능
- 이렇게 전달한 데이터는 이벤트와 연결된 부모 컴포넌트의 핸들러 함수의 인자로 사용 가능
- pass props와 마찬가지로 동적인 데이터도 전달 가능

```html
<!-- MyChild.vue -->
<template>
  <div>
    ...
    <button @click="ChildToParent">클릭!</button>
  </div>
</template>

<script>
export default {
  ...
  methods: {
    ChildToParent() {
      this.$emit('child-to-parent', 'child data')
    },
  }
}
</script>
```

```html
<!-- MyComponent.vue -->
<template>
    ...
    @child-to-parent="parentGetEvent"
    />
  </div>
</template>

<script>
export default {
  ...
  methods: {
    parentGetEvent(inputData) {
      console.log("자식 컴포넌트에서 발생한 이벤트!")
      console.log(`하위 컴포넌트에서 ${inputData}를 받음`)
    }
  }
}
</script>
```

---

### 4. Emit with dynamic Data

- pass props와 마찬가지로 동적인 데이터도 전달 가능

```html
<!-- MyChild.vue -->
<template>
  <div>
    ...
    <input type="text" v-model="childinputData" @keyup.enter="ChildInput">
  </div>
</template>

<script>
export default {
  ...
  data: function() {
    return {
      childinputData: null
    }
  },
  methods: {
    ChildInput() {
      this.$emit('child-input', this.childinputData)
      this.childinputData = ""
    }
  }
}
</script>
```

```html
<!-- MyComponent.html -->
<template>
  <div class="border">
    <MyChild
    @child-input="getDynamicData"
    />
  </div>
</template>

<script>
export default {
  ...
  methods: {
    getDynamicData(inputData){
      console.log(`하위 컴포넌트에서 ${inputData}를 받음`)
    }
  }
}
</script>
```

---

### 5. Emit Event Composition API

위 예시 코드를 Vue3의 Composition API로 변경하면 다음과 같다

```html
<!-- MyChild.vue -->
<template>
  <div>
    ...
    <input type="text" v-model="childinputData" @keyup.enter="childInput">
  </div>
</template>

<script setup>
import { ref, emit, defineEmits } from 'vue';

const childinputData = ref(null);

const emit = defineEmits(['child-input']);

const childInput = () => {
  if (childinputData.value) {
    emit('child-input', childinputData.value);
    childinputData.value = "";
  }
};
</script>
```

```html
<!-- MyComponent.vue -->
<template>
  <div class="border">
    <MyChild @child-input="getDynamicData" />
  </div>
</template>

<script setup>
import { onMounted } from 'vue';

const getDynamicData = (inputData) => {
  console.log(`하위 컴포넌트에서 ${inputData}를 받음`);
};
</script>
```