---
title: 'Vuex의 시작'
date: '2020-03-27'
category: 'Vue'
---

# Vuex란?

Vuex는 Vue와 함께 사용하기 위한 `상태 관리 패턴` 라이브러리 이다.

react에서 사용하는 redux, context API, mobx와 같다고 생각하면 된다.

앱의 모든 구성 요소에서 엑세스할 수 있는 중앙 집중식 데이터 저장소를 만드는 것이다.

### vuex 설치

vue-cli를 설치할 때, vuex도 같이 설정해준다.

### scss 사용

```
npm install --save-dev node-sass sass-loader
```

scss 관련 모듈을 설치해준다.

사용방법은 매우 간단하다.

```js
<style lang='scss'>
  // 바로 작성 가능
</style>

// style 파일을 따로 만들어 import
<style lang='scss' src='@/styles/app.scss"></style>
```

`@`는 src 폴더를 의미한다.

# store 생성

![](https://images.velog.io/images/jotang/post/b85f5fe4-58f3-4cdd-9d51-806bf41b4b43/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-03-27%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%202.38.57.png)

store 폴더를 생성하고, index.js를 생성한다.

- store의 기본 구조

```js
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

export const store = new Vuex.Store({
  state: {},
  mutations: {},
  actions: {},
  modules: {},
});
```

> state는 Vue에서 data를 정의해주는 것과 같다고 생각하면 된다.

Store를 앱에서 사용하기 위해서 저장소 정보를 상수 store에 할당한 뒤에 export한다.

이렇게 내보낸 정보는 `main.js`에서 받아 사용하게 된다. Store에서 작성한 `Vue.use(Vuex)`에 의해서 `store` 속성을 사용할 수 있게된다.

```js
// main.js
import Vue from 'vue';
import App from './App.vue';
import { store } from './store';

Vue.config.productionTip = false;

new Vue({
  store,
  render: h => h(App),
}).$mount('#app');
```

store를 생성하였기 때문에 각 컴포넌트가 실행될 때, props를 사용하지 않아도 된다.

```js
<script>
export default {
// 기존 props는 삭제
// props: ['fruits']
  computed: {
    fruits() {
      return this.$store.state.fruits;
    }
  }
};
</script>

```

`computed` 속성을 추가하여 Store에서 데이터를 가져와서 바인딩한다.
저장소에서 State가 변경되면, computed가 변경되고, DOM 업데이트가 된다.

### getters

state를 계산된 상태로 사용할 경우 `getters`를 사용할 수 있다.

`computed`속성과 비슷하다

다만, store의 state를 컴포넌트에서 computed에서 직접 계산하여 사용하지 않도록 주의해야한다

### mutations

store의 state를 변경하는 방법
