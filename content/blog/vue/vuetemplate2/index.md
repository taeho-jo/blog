---
title: 'Vue template2'
date: '2020-03-21'
category: 'Vue'
---

# 디렉티브(Directives)

##### 어제에 이어서 남아있는 디렉티브에 대해서 알아보도록 하겠습니다!!

## 8. v-on

일반 엘리먼트에 사용되면 **기본 DOM이벤트**만 받고, 사용자 정의 컴포넌트에서 사용될 때 해당 하위 컴포넌트에서 생성된 **사용자 정의 이벤트**를 받는다.

약어는 `@`

### 이벤트 청취

`v-on` 디렉티브를 이용하면, DOM이벤트를 듣고 트리거 될 때 JavaScript를 실행할 수 있다.

```html
<div id="example-1">
  <button v-on:click="counter += 1">Add 1</button>
  <p>위 버튼을 클릭한 횟수는 {{ counter }} 번 입니다.</p>
</div>
```

```js
var example1 = new Vue({
  el: '#example-1',
  data: {
    counter: 0,
  },
});
```

### 메소드 이벤트 핸들러

`v-on` 의 속성 값으로 이벤트 핸들러 로직을 직접 입력하게 된다면, 코드는 굉장히 복잡해지게 된다.
때문에 `v-on`을 통해 이벤트가 발생할 때, 메소드의 이름을 받는다.

```html
<div id="example-2">
  <!-- `greet`는 메소드 이름으로 아래에 정의되어 있습니다 -->
  <button v-on:click="greet">Greet</button>
</div>
```

```js
var example2 = new Vue({
  el: '#example-2',
  data: {
    name: 'Vue.js',
  },
  // 메소드는 `methods` 객체 안에 정의합니다
  methods: {
    greet: function(event) {
      // 메소드 안에서 사용하는 `this` 는 Vue 인스턴스를 가리킵니다
      alert('Hello ' + this.name + '!');
      // `event` 는 네이티브 DOM 이벤트입니다
      if (event) {
        alert(event.target.tagName);
      }
    },
  },
});

// 또한 JavaScript를 이용해서 메소드를 호출할 수 있습니다.
example2.greet(); // => 'Hello Vue.js!'
```

### 인라인 메소드 핸들러

메소드 이름을 직접 바인딩 하는 대신 인라인 JavaScript 구문에 메소드를 사용할 수 있다.

```html
<div id="example-3">
  <button v-on:click="say('hi')">Say hi</button>
  <button v-on:click="say('what')">Say what</button>
</div>
```

```js
new Vue({
  el: '#example-3',
  methods: {
    say: function(message) {
      alert(message);
    },
  },
});
```

인라인 메소드 핸들러에서 원본 DOM 이벤트에 접근해야 할 수도 있다. 특별한 `$event` 변수를 사용해 메소드에 전달할 수도 있다.

```html
<button v-on:click="warn('Form cannot be submitted yet.', $event)">
  Submit
</button>
```

```js
// ...
methods: {
  warn: function (message, event) {
    // 이제 네이티브 이벤트에 액세스 할 수 있습니다
    if (event) event.preventDefault()
    alert(message)
  }
}
```

### 이벤트 수식어

이벤트 핸들러 내부에서 `event.preventDefault()` 또는 `event.stopPropagation()`을 호출하는 것은 매우 일반적인 일이다. 메소드 내에서 쉽게 이 작업을 할 수 있지만, DOM 이벤트 세부 사항을 처리하는 대신 데이터 로직에 대한 메소드만 사용할 수 있다면 코드는 더욱 간결해질 것이다.

이 문제를 해결하기 위해서, **이벤트 수식어**를 제공한다.

- `.stop`
- `.prevent`
- `.capture`
- `.self`
- `.once`
- `.passive`

```html
<!-- 클릭 이벤트 전파가 중단됩니다 -->
<a v-on:click.stop="doThis"></a>

<!-- 제출 이벤트가 페이지를 다시 로드 하지 않습니다 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 수식어는 체이닝이 가능 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 수식어만 사용할 수 있습니다 -->
<form v-on:submit.prevent></form>

<!-- 이벤트 리스너를 추가할 때 캡처모드를 사용합니다 -->
<!-- 즉, 내부 엘리먼트를 대상으로 하는 이벤트가 해당 엘리먼트에서 처리되기 전에 여기서 처리합니다. -->
<div v-on:click.capture="doThis">...</div>

<!-- event.target이 엘리먼트 자체인 경우에만 트리거를 처리합니다 -->
<!-- 자식 엘리먼트에서는 안됩니다 -->
<div v-on:click.self="doThat">...</div>

<!-- 클릭 이벤트는 최대 한번만 트리거 됩니다. -->
<a v-on:click.once="doThis"></a>

<!-- 스크롤의 기본 이벤트를 취소할 수 없습니다. -->
<div v-on:scroll.passive="onScroll">...</div>
```

- 네이티브 DOM 이벤트에 독점적인 다른 수식어와는 달리, `.once` 수식어는 컴포넌트 이벤트에서도 사용할 수 있다.

- `.passive` 는 특히 모바일 환경에서 성능향상에 도움이 된다. 예를 들어, 브라우저는 핸들러가 `event.preventDefault()`를 호출할지 알지 못하므로 프로세스가 완료된 후 스크롤 한다. `.passive` 수식어는 이벤트가 기본 동작을 멈추지 않는다는 것을 브라우저에 알릴 수 있다.

> .passive와 .prevent를 함께 사용하면 안된다. .prevent는 무시되고 브라우저는 오류를 발생시키기 때문이다. .passive는 이벤트의 기본 행동을 무시하지 않기를 원하는 브라우저와 상호작용한다는 사실을 기억해야한다.

> 관련 코드가 동일한 순서로 생성되기 때문에 수식어를 사용할 때 순서를 지정해야 합니다. 다시 말해 v-on:click.prevent.self 를 사용하면 모든 클릭을 막을 수 있고, v-on:click.self.prevent 는 엘리먼트 자체에 대한 클릭만 방지한다.

## 9. v-bind

HTML 태그의 기본 속성과 뷰 데이터 속성을 연결한다.

약어는 `:`

동적으로 하나 이상의 컴포넌트 속성 또는 표현식을 바인딩하게 된다.
class 또는 style 속성을 묶는 데 사용될 때, Array나 Objects와 같은 추가 값 유형을 지원한다.

전달인자 없이 사용하면 속성 이름-값 쌍을 포함하는 객체를 바인딩 하는데 사용할 수 있다. 이 때에는 class나 style은 Array와 Objects를 지원하지 않는다.

```html
<!-- 속성을 바인딩 합니다. -->
<img v-bind:src="imageSrc" />

<!-- dynamic attribute name (2.6.0+) -->
<button v-bind:[key]="value"></button>

<!-- 약어 -->
<img :src="imageSrc" />

<!-- shorthand dynamic attribute name (2.6.0+) -->
<button :[key]="value"></button>

<!-- with inline string concatenation -->
<img :src="'/path/to/images/' + fileName" />

<!-- 클래스 바인딩 -->
<div :class="{ red: isRed }"></div>
<div :class="[classA, classB]"></div>
<div :class="[classA, { classB: isB, classC: isC }]">
  <!-- 스타일 바인딩 -->
  <div :style="{ fontSize: size + 'px' }"></div>
  <div :style="[styleObjectA, styleObjectB]"></div>

  <!-- 속성 객체 바인딩 -->
  <div v-bind="{ id: someProp, 'other-attr': otherProp }"></div>

  <!-- prop 수식어를 사용하는 DOM 속성 바인딩 -->
  <div v-bind:text-content.prop="text"></div>

  <!-- 속성 바인딩. 컴포넌트에서 "prop"를 선언 해야 합니다.  -->
  <my-component :prop="someThing"></my-component>

  <!-- 자식 컴포넌트와 공통으로 사용하는 부모 props를 전달합니다 -->
  <child-component v-bind="$props"></child-component>

  <!-- XLink -->
  <svg><a :xlink:special="foo"></a></svg>
</div>
```

### 수식어

- `.prop` : 속성 대신 DOM속성으로 바인딩. 만약 태그가 컴포넌트라면 `.props`는 컴포넌트의 `$el`에 속성을 추가

- `.camel` : 카멜 케이스로 변환

```html
<svg :view-box.camel="viewBox"></svg>
```

- `.sync` : 바인딩 된 값을 업데이트하기 위한 v-on을 확장.

## 10. v-model

폼(form) 에서 주로 사용되는 속성이다. 폼에 입력한 값을 뷰 인스턴스의 데이터와 즉시 동기화 시켜준다. 화면에 입력된 값을 저장하여 서버에 보내거나, watch와 같은 고급 속성을 이용하여 추가 로직을 수행할 수 있다. `input`, `select`, `textarea` 태그에만 사용할 수 있다.

```html
<input v-model="message" placeholder="여기를 수정해보세요" />
<p>메시지: {{ message }}</p>

<!-- 여러줄을 가진 문장 -->
<span>여러 줄을 가지는 메시지:</span>
<p style="white-space: pre-line">{{ message }}</p>
<br />
<textarea v-model="message" placeholder="여러줄을 입력해보세요"></textarea>
```

### 수식어

- `.lazy` : input 대신 `change` 이벤트를 듣는다.

- `.number` : 문자열을 숫자로 변경

- `.trim` : 입력에 대한 trim을 한다

## 11. v-slot

자식 컴포넌트의 엘리먼트를 부모에서 지정할 때 사용
간단하게 `name-slot`과 `slot-scope`를 합친 문법. name-slot / slot-scope에 대해서는 다음에 자세히 알아보도록 하고 오늘은 v-slot에 대해 알아보겠다.

약어 `#`

```html
<!-- BaseButton.vue -->
<button>
  <slot>기본값</slot>
</button>

<!-- Parent -->
<BaseButton>
  <!-- 슬롯이 들어갑니다. -->
  <span>저장하기</span>
</BaseButton>

<!-- 렌더링 결과 -->
<button>
  <span>저장하기</span>
</button>
```

BaseButton 컴포넌트 템플릿 내에 엘리먼트를 사용하면 슬롯으로 지정된다.
컴포넌트에서 구체적인 요소를 선언하지 않고 부모에게 위임한다.
slot은 컴포넌트의 재사용성을 높여주는 중요한 기능.

공식문서는 `v-slot`을 사용하고, `named-slot`과 `slot-scope` 의 사용은 권장하지 않고 있다.

```html
<!-- Parent -->
<div>
  <BaseModal>
    <template v-slot:header="slotProps">
      <h1>모달 제목</h1>
      <button @click="slotProps.close">닫기</button>
      {{ slotProps }}
      <!-- { hello: 'hello' } -->
    </template>
    <template v-slot:content>
      <p>모달의 컨텐츠입니다.</p>
    </template>
  </BaseModal>
</div>
```

주의사항은 v-slot은 반드시 `template`로 감싸서 사용해야한다.

## 12. v-pre

특정 엘리먼트를 무시하는데에 사용된다. 해당 엘리먼트에는 지시문이 없다는 것을 인식하게 되어서 그 엘리먼트 내부의 자식 엘리먼트들을 신경쓰지 않고 그냥 건너뛰게 된다. 따라서 컴파일 속도가 빨라진다.

따로 표현식은 필요하지 않다.

```html
<span v-pre>{{ 이 부분은 컴파일 되지 않습니다 }}</span>
```

## 13. v-cloak

JavaScript가 실행 되기전에, Vue 인스턴스가 제대로 준비되기 전 까지 HTML 코드를 숨기고 싶을 때, `v-cloak` 디렉티브를 사용.
한 마디로 vue에서 렌더링 되기 이 전에 브라우저에서 감추는 역할은 한다.

```js
[v-cloak] {
  display: none;
}
// 먼저 기능 구현을 위해 위와 같이
// v-cloak에 display: none을 선언
```

```html
<!-- 적용할 태그에 작성 -->
<div v-if="false" v-cloak>TEST</div>
```

꼭 사용하지 않아도 결과적으로 본다면 차이는 없다. 다만 렌더딩 과정에서 화면이 깜박거릴 수 있고 {{ }}를 사용한 표현식이 미리 보이거나 안 보이도록 설정된 요소들도 잠깐 나타났다 사라지게되는 문제가 있다. 이런 이유로 가급적 v-cloak을 사용하는 것이 좋을 것이다.

## 14. v-once

표현식이 필요하지 않다.

엘리먼트 및 컴포넌트를 한 번만 렌더링 한다. 후속 렌더링에서 엘리먼트 및 컴포넌트와 모든 하위 엘리먼트는 정적으로 처리되면서 건너 뛰게 된다. 업데이트 성능을 최적화하는데 사용한다.

```html
<!-- 단일 엘리먼트 -->
<span v-once>This will never change: {{msg}}</span>

<!-- 자식 엘리먼트를 포함하는 엘리먼트 -->
<div v-once>
  <h1>comment</h1>
  <p>{{msg}}</p>
</div>

<!-- 컴포넌트 -->
<my-component v-once :comment="msg"></my-component>

<!-- v-for 디렉티브 -->
<ul>
  <li v-for="i in list" v-once>{{i}}</li>
</ul>
```
