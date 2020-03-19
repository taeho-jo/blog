---
title: 'Vue Lifecycle hooks'
date: '2020-03-19'
category: 'Vue'
---

# Vue Instance

모든 Vue는 새로운 Vue 인스턴스를 만드는 것 부터가 시작.

```js
const vm = new Vue({
  //옵션
});

console.log(vm);
```

인스턴스를 생성하고 나면 인스턴스 안에 어떤 속성과 API가 있는 console창에서 확인 할 수 있다.

![](https://images.velog.io/images/jotang/post/9e5cef8d-15bd-4183-9f4d-b5e6a9aed387/image.png)

### Instance의 속성, API 들

인스턴스에서 사용할 수 있는 속성과 API는 아래와 같다.

```js
new Vue ({
 el: ,
 template: ,
 data: ,
 methods: ,
 created: ,
 watch: ,
})
```

- **el** : 인스턴스가 그려지는 화면의 시작점(특정 HTML 태그)
- **template** : 화면에 표시할 요소(HTML, CSS 등)
- **data** : 뷰의 반응성이 반영된 데이터 속성
- **methods** : 화면의 동작와 이벤트 로직을 제어하는 methods
- **created** : 뷰의 LifeCycle과 관련된 속성
- **watch** : 데이터에서 정의한 속성이 변화했을 때, 추가적인 동작을 수행할 수 있게 정의하는 속성

앞으로 공부해나가면서 자세하게 알아보도록 하겠습니다.

### Instance Lifecycle

vue의 인스턴스가 생성되어 소멸되기까지 거치는 과정을 의미한다. 간단하게 살표보면

- data 속성의 초기화 및 관찰 (Reactivity 주입)
- 뷰 템플릿 코드 컴파일(Virtual DOM -> DOM)
- 인스턴스를 DOM에 부착

### Lifecycle Diagram

![](https://images.velog.io/images/jotang/post/3c079f25-7552-47ac-a891-7868cace2962/vue_lifecycle.png)

Vue 인스턴스는 크게 `생성(create)` -> `DOM부착(mount)` -> `업데이트(update)` -> `사라짐(destory)` 4가지 과정을 거친다.

![](https://joshua1988.github.io/vue-camp/assets/img/lifecycle.dcbe29f6.png)

# Lifecycle hooks

Vue의 라이프 사이클을 이해해야 하는 이유는 라이프 사이클 훅 때문.
라이플 사이클 훅으로 인스턴스의 특정 시점에 원하는 로직을 구현할 수 있다.

- 모든 라이프 사이클 훅은 자동적으로 `this` context를 인스턴스에 바인딩 한다. (데이터, 계산된 속성 및 메소드에 접근 가능)
- 화살표 함수를 사용해 라이프 사이클 **메소드를 정의하면 안된다**.

## 1. beforeCreate

```js
var app = new Vue({
    el: '#app',
    data() {
        return {
            msg: 'hello';
        }
    },
    beforeCreate(function() {
        console.log(this.msg); // undefined
    })
})
```

- 인스턴스가 초기화 된 직후 실행된다.(component가 DOM에 추가되기 전)

- `this.$el`에 접근할 수 없고, data, event 에도 접근하기 전이기 때문에 `data, methods에도 접근할 수 없다`.

## 2. created

```js
var app = new Vue({
    el: '#app',
    data() {
        return {
            msg: 'hello';
        }
    },
    created(function() {
        console.log(this.msg); // hello
    })
})
```

- data를 반응형으로 추척할 수 있게 된다.

- `computed`, `methods`, `watch` 에 접근이 가능하게 된다.

- DOM에는 추가되지 않은 상태

- data에 직접 접근이 가능하기 때문에, 외부에서 받아오는 값들로 data를 세팅하거나, 이벤트 리스너를 선언해야 한다면 created 단계에서 하는 것이 가장 적절하다.

## 3. beforeMount

```js
var app = new Vue({
    el: '#app',
    beforeMount(function() {
        console.log('beforeMount');
    })
})
```

- DOM에 추가되기 직전에 호출된다.

- 가상 DOM에는 생성되어 있으나, 실제 DOM에는 추가되지 않은 상태

## 4. mounted

```js
var app = new Vue({
    el: '#app',
    mounted(function() {
        console.log('mounted');
    })
})
```

- 일반적으로 **가장 많이 사용되는** 라이프 사이클 훅.

- 실제 DOM에 추가되고 난 이후에 실행되기 때문에, `this.$el`, `data`, `computed`, `methods`, `watch` 등 모든 요소에 접근이 가능하다.

![](https://i.stack.imgur.com/uTF2n.png)

부모와 자식 component 간의 mounted 훅 순서는 위의 이미지 처럼 `부모 component의 mounted 훅`은 항상 `자식 component의 mounted 훅 이후에 발생`한다는 것을 알 수 있다.

```js
var app = new Vue({
    el: '#app',
    mounted(function() {
        this.$nextTick(function() {
            // 모든 화면이 렌더링된 후 실행합니다.
        })
    })
})
```

자식 components가 서버에서 비동기로 데이터를 받아오는 경우처럼, 부모의 mounted 훅이 모든 자식 components가 마운트 된 상태를 보장하지는 않는다.
이때에는 `this.$nextTick`을 이용하여 모든 화면이 렌더링 된 이후에 실행되게 하여 mounted 상태를 보장할 수 있다.

## 5. beforeUpdate

```js
var app = new Vue({
    el: '#app',
    beforeUpdate(function() {
        console.log('beforeUpdate');
    })
})
```

component에서 사용되는 data의 값이 변함에 따라, DOM에도 변화를 주어 렌더링을 다시 해야할 때에, **변화 직전에 호출되는 것이 beforeUpdate 훅**이다.

- 변하는 값을 가상 DOM에 렌더링하기 전이지만, 이 변하는 값을 이용하여 작업할 수 있다.

- 이 beforeUpdate 훅에서 값들을 추가적으로 변화시키더라도 렌더링을 추가로 하지는 않는다.

## 6. updated

```js
var app = new Vue({
    el: '#app',
    updated(function() {
        console.log('beforeUpdate');
    })
})
```

가상 DOM을 렌더링하고 **실제 DOM이 변경된 이후에 호출되는 것이 updated 훅**이다.

변경된 데이터가 실제 DOM에 적용이 된 상태. 변경된 값들을 DOM을 이용해 접근하고 싶다면, updated 훅이 가장 적절하다.

- 하지만, 데이터를 변경하는 것은 무한 루프를 일으킬 수 있으므로, 직접적으로 데이터를 바꾸어서는 안된다.

- mounted 훅과 마찬가지로, `this.$nextTick`을 이용하여, 모든 화면에 업데이트 된 이후의 상태를 보장할 수 있다.

## 7. beforeDestory

```js
var app = new Vue({
    el: '#app',
    beforeDestroy(function() {
        console.log('beforeDestroy');
    })
})
```

해당 **인스턴스가 없어지기 직전에 beforeDestory 훅이 호출**된다.
아직 사라지기 전이기 때문에, 인스턴스의 모든 속성에 접근이 가능하다.

- 이벤트 리스너를 해체하는 등 인스턴스가 사라지기 전에 해야할 일들을 처리하면 된다.

## 8. destoryed

```js
var app = new Vue({
    el: '#app',
    destroyed(function() {
        console.log('destroyed');
    })
})
```

인스턴스가 사라지고 난 직후에 destoryed 훅이 호출된다.
없어진 다음이기 때문에, 인스터스 속성에 접근할 수 없다. 하위 Vue 인스턴스 역시 삭제된다.

##### 오늘은 template까지 하려고 하였으나, 다른 일정이 있어 Lifecycle hooks까지만 하게 되었습니다.

##### 내일은 template부터 다른 부분들 까지 더 공부하도록 하겠습니다.

##### 틀렸거나 부족한 개념은 알려주시면 감사드립니다!
