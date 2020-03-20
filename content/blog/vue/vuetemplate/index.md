---
title: 'Vue template1'
date: '2020-03-20'
category: 'Vue'
---

# Vue template

vue의 template 문법이란 vue로 화면을 조작하는 방법을 의미한다. template 문법은 크게 `데이터 바인딩`과 `디렉티브`로 나뉩니다

- 실제 화면에서 보일 **HTML 틀** 이라고 할 수 있다.

- 일정한 규칙에 의해서 해석되는 HTML 문서

- 만들어진 데이터를 이용해 템플릿 블럭내 DOM조작이 가능하다.

- 주로 디렉티브로 이루어져 DOM 엘리먼트에 속성을 추가하는 것을 의미한다.

## 데이터 바인딩

### 보간법(Interpolation)

#### 1. 문자열

데이터 바인딩의 가장 기본적인 형태는 `Mustache` 구문이다. ( 이중 중괄호 )

```html
<span>메세지: {{ msg }}</span>
```

Mustache 태그는 해당 데이터 객체의 `msg` 속성 값으로 대체. 데이터 객체의 msg의 값이 바뀔 때 마다 갱신된다.

#### 2. 원시 HTML

위에서 보았던 mustaches는 일반 text로 데이터를 해석한다. 실제 HTML 코드를 출력하려면, v-html 디렉티브를 사용해야한다.

```html
<p>Using mustaches: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```

span의 내용은 rawHtml로 대체된다. 이 때 데이터 바인딩은 무시된다. vue는 문자열 기반 템플릿 엔진이 아니기 때문에 v-html을 이용하여 템플릿을 사용할 수 없고, 대신에 컴포넌트는 UI 재사용 및 구성을 위한 기본 단위로 사용하는 것을 추천한다.

#### 3. 속성

mustaches는 HTML 속성 값으로 사용할 수 없다. 그 대신에 `v-bind`를 사용해야한다.

```html
<div v-bind:id="dynamicId"></div>
```

boolean 속성을 사용할 때, 단순히 값이 true인 경우 v-bind가 조금 다르게 동작한다.

```html
<button v-bind:disabled="isButtonDisabled">Button</button>
```

isButtonDisabled가 null, undefined, false의 값을 가지게 되면 disabled속성은 렌더링 된 button 엘리먼트에 포함되지 않는다.

#### 4. JavaScript 표현식 사용

vue는 모든 데이터 바인딩 내에서 JavaScript 표현식의 모든 기능을 지원한다.

```js
{
  {
    number + 1;
  }
}
{
  {
    ok ? '좋아요' : '싫어요';
  }
}
{
  {
    data
      .split('')
      .reverse()
      .join('');
  }
}
<div v-bind:id="'list-' + id"></div>;
```

하지만, 각 바인딩에는 하나의 단일 표현식만 포함될 수 있다.

```js
// 아래와 같이 작성하면 안된다.

{
  {
    let a = 1;
  }
}

{
  {
    if (ok) {
      return message;
    }
  }
}

// 표현식이 아니라 구문을 작성하면 안된다.
// 조건문은 작동하지 않고, 삼항 연산자를 사용해야 한다.
```

## 디렉티브(Directives)

디렉티브는 뷰로 화면의 요소를 조금 더 쉽게 조작하기 위한 문법이다.
화면 조작에서 주로 사용되는 방식들을 모아 디렉티브 형태로 제공하고 있다.
예를 든다면, 특정 속성 값에 따라 화면의 영역을 표시하거나 표시하지 않을 수 있다.

```html
<!-- show라는 데이터 값에 따라 출력되거나 되지 않는 코드 -->

<div>Hello <span v-if="show">Vue.js</span></div>
```

```js
new Vue({
  data: {
    show: false,
  },
});
```

### 전달인자

일부 디렉티브는 `:` 을 통해 `전달인자`를 사용할 수 있다.
예를 든다면, v-bind 디렉티브는 반응적으로 HTML 속성을 갱신하는데 사용된다.

```html
<a v-bind:href="url">...</a>

<a v-on:click="doSomething">...</a>
```

href는 전달인자로, 엘리먼트의 href 속성을 표현식 url의 값에 바인드하는 v-bind 디렉티브에게 알려준다.

v-on은 DOM 이벤트를 수신한다. 전달인자는 이벤트를 받을 이름이다.

### 동적 전달인자

> 2.6.0+ 에서 추가

JavaScript 표현식을 대괄호로 묶어 디렉티브의 전달인자로 사용하는 것도 가능하다.

```html
<a v-bind:[attributeName]="url"> ... </a>

<!-- JavaScript형식으로 동적 변환되어, -->
<!-- 변환된 결과가 전달인자의 최종적인 value로 사용된다. -->
<!-- "href"라는 값을 가진 attributeName 데이터 속성이 있는 경우 -->
<!-- v-bind:href 와 똑같다. -->
```

이벤트명에 바인딩 할 때에도 동적 전달인자를 활용할 수 있다.
eventName이라는 데이터 속성의 값이 "focus" 라면 `v-on:[EventName]은 v-on:focus와 동일`하다

```html
<a v-on:[eventName]="doSomething"> ... </a>
```

하지만 동적 전달인자에는 몇 가지 제약 사항들이 있다.

- `null`을 제외하고는 string으로 변환될 것을 예측하고, 특수 값인 null은 명시적으로 바인딩을 제거하는데 사용된다. 그 외의 경우, string이 아닌 값은 경고를 출력하게 된다.

- 문자상의 제약이 있다. 스페이스와 따옴표 같은 문자는 HTML의 속성명으로 적합하지 않기 때문이다.

- in-DOM 템플릿을 사용할 때`(탬플릿이 HTML파일에 직접 쓰여진 경우)`에는, 브라우저가 모든 속성명을 소문자로 만들기 때문에 대문자의 사용을 피하는 것이 좋다

```html
<!-- 잘못 사용된 예시(스페이스와 따음표 사용) -->
<a v-bind:['foo' + bar]='value'>...</a>

<!-- in-DOM에서는 v-bind:[someArr]이 v-bind:[somearr]로 변환 -->
<!-- "somearr"에 속성이 없는 경우, 작동하지 않는다 -->
<a v-bind:[someArr]="value">...</a>
```

### 수식어

`.` 으로 표시되는 특수 접미사.

디렉티브를 특별한 방법으로 바인딩 해야하는 것을 나타낸다.
`.prevent` 수식어는 이벤트에서 `event.preventDefault()`를 호출하도록 `v-on`디렉티브에게 알려준다.

```html
<form v-on:submit.prevent="onSubmit">...</form>
```

추후에 `v-on`과 `v-model`에서 자세히 살펴보도록 하겠다.

### 약어

`v-` 는 vue에서 특정 속성을 식별하기 위한 시각적인 역할을 하고있다.
vue를 통해 기존의 마크업에 동적인 동작을 적용할 때 유용하지만, 자주 사용되는 디렉티브에서는 너무 길다고 느껴질 수 있다.

vue가 모든 템플릿을 관리하는 SPA를 만들 때 `v-`의 필요성또한 떨어진다.

따라서 가장 자주 사용되는 두 개의 디렉티브인 `v-bind`와 `v-on`에는 약어를 제공하고 있다.

#### v-bind 약어

```html
<!-- 전체 문법 -->
<a v-bind:href="url">...</a>

<!-- 약어 -->
<a :href="url">...</a>

<!-- 동적 전달인자 -->
<a :[key]="url">...</a>
```

#### v-on 약어

```html
<!-- 전체 문법 -->
<a v-on:click="doing">...</a>

<!-- 약어 -->
<a @click="doing">...</a>

<!-- 동적 전달인자 -->
<a @[event]="doing">...</a>
```

##### 지금부터는 vue의 디렉티브에 대해서 더 자세히 알아보도록 하겠다.

### Vue 디렉티브의 종류

디렉티브의 종류는 14가지가 있다.
오늘은 절반이 7가지에 대해서 알아보고 나머지 7가지는 내일 알아보도록하겠다.
오늘 알아볼 디렉티브는
`v-text`, `v-html`, `v-show`, `v-if`, `v-else`, `v-else-if`, `v-for`

내일 알아볼 디렉티브는
`v-on`, `v-bind`, `v-model`, `v-slot`, `v-pre`, `v-cloak`, `v-once`

밑에서 기본적인 것들에 대해서 알아보도록 하겠다.

#### 1. v-text

`mustache` 와 비슷하게 작동하지만, 중괄호 대신 디렉티브를 이용하여 데이터를 바인딩 한다.

```html
<span v-text="message"></span>
<!-- 아래와 같다 -->
<span>{{ message }}</span>
```

#### 2. v-html

HTML 문자열을 그대로 바인딩 한다.(XSS 공격에 유의)

```html
<span v-html="message"></span>

<script>
  new Vue({
    data: {
      message: 'Hi<br>Hi',
    },
  });
</script>
```

#### 3. v-show

엘리먼트들을 조건부로 표시하기 위한 옵션이다.

```html
<h1 v-show:"hello">안녕하세요!</h1>
```

`v-show` 가 있는 엘리먼트는 항상 렌더링 되고, DOM에 남아있다. 단순히 엘리먼트에 display CSS속성을 토글한다.

> v-show는 template 구문을 지원하지 않고, v-else와도 작동하지 않는다.

#### 4. v-if

표현식의 true false를 기반으로 조건부 렌더링을 한다.

```html
<h1 v-if="yes">그래!</h1>

<!-- v-else와 함께 "else 블록"을 추가하는 것도 가능 -->
<h1 v-if="yes">그래!</h1>
<h1 v-else>아냐!</h1>
```

**v-if 갖는 조건부 그룹 만들기**

하나 이상의 엘리먼트를 조건부렌더링하기 위해서는 template 엘리먼트에 v-if를 사용할 수 있다.

```html
<template v-if="hello">
  <h1>인사말</h1>
  <p>안녕~</p>
  <p>반가워~</p>
</template>
```

#### 5. v-else

특별한 표현식은 필요가 없다.
다만, `v-else` 는 `v-if` 또는 `v-else-if` 바로 뒤에 있어야한다. 그렇지 않으면 인식하지 못한다.

```html
<div v-if="Math.random() > 0.5">
  난 0.5보다 커
</div>
<div v-else>
  난 0.5보다 작아
</div>
```

#### 6. v-else-if

v-if에 대한 `else if 블럭`을 나타낸다. **체이닝이 가능**하다.

```html
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>
```

`v-else`와 마찬가지로, `v-if` 또는 `v-else-if` 바로 뒤에 있어야한다.

#### 6.1 v-if & v-show

- `v-if`는 토글하는 동안 제거되고, 다시 만들어지기 때문에 진짜 조건부 렌더링.
- 초기 렌더링에서 조건이 false라면 아무것도 하지 않는다. 조건 블록이 true가 될 때 까지 렌더링 하지 않는다.
- 이에 반해, `v-show`는 훨씬 단순하다.
- css기반 토글만으로 초기 조건에 관계 없이 항상 렌더링 된다.
- `v-if`는 toggle 비용이 높고, `v-show`는 초기 렌더링 비용이 높다. 따라서 자주 바꾸기를 원한다면 `v-show`를, 런타임 시 조건이 바뀌지 않는다면 `v-if`를 권장한다.

#### 6.2 key를 이용한 재사용 가능한 엘리먼트 제어

vue는 가능한 효율적으로 엘리먼트를 렌더링하려고 시도합니다. 따라서 처음부터 렌더링을 하지 않고 재사용하여 렌더링을 할 때도 있습니다.
아래 예시를 통해서 자세히 알아보도록 하겠습니다.

```html
<template v-if="loginType === 'username'">
  <label>사용자 이름</label>
  <input placeholder="사용자 이름을 입력하세요" />
</template>
<template v-else>
  <label>이메일</label>
  <input placeholder="이메일 주소를 입력하세요" />
</template>
```

![](https://images.velog.io/images/jotang/post/9ce892e9-2f74-4166-bcc0-e39702087a25/image.png)

위 코드에서 `loginType`을 바꾸어도 사용자가 입력한 내용은 바뀌지 않습니다. 두 템플릿은 같은 요소를 사용하기 때문입니다. `input`태그는 새로 렌더링되지 않고, `placeholder`만 변경됩니다.

이 때, `이 두 엘리먼트는 다른 것이므로 다시 사용하지 마시오` 라고 알리는 방법을 vue는 제공하고 있는데, 그것은 유일한 값으로 `key` 속성을 추가하는 것이다.

```html
<template v-if="loginType === 'username'">
  <label>사용자 이름</label>
  <input placeholder="사용자 이름을 입력하세요" key="username-input" />
</template>
<template v-else>
  <label>이메일</label>
  <input placeholder="이메일 주소를 입력하세요" key="email-input" />
</template>
```

label 엘리먼트는 key 속성이 없기 때문에 여전히 효율적으로 재사용 된다.

#### 7. v-for

원본 데이터를 기반으로 엘리먼트 또는 템플릿 블록을 여러번 렌더링한다.

#### 7.1 v-for로 배열 매핑하기

v-for 디렉티브를 이용하여 배열을 기반으로 리스트를 렌더링 할 수 있다.
`item in items`라는 형태의 특별한 문법이 필요하다.

- `items`는 원본 데이터 배열
- `item`은 반복되는 배열 엘리먼트의 별칭

```html
<ul id="example-1">
  <li v-for="item in items">
    {{ item.message }}
  </li>
</ul>
```

```js
var example1 = new Vue({
  el: '#example-1',
  data: {
    items: [{ message: 'Foo' }, { message: 'Bar' }],
  },
});
```

결과는

- Foo
- Bar

  라고 출력됩니다.

v-for 블록은 부모 범위 속성에 접근할 수 있습니다. 또 두번째 인자로 인덱스를 사용할 수 있습니다.

```html
<ul id="example-2">
  <li v-for="(item, index) in items">
    {{ parentMessage }} - {{ index }} - {{ item.message }}
  </li>
</ul>
```

```js
var example2 = new Vue({
  el: '#example-2',
  data: {
    parentMessage: 'Parent',
    items: [{ message: 'Foo' }, { message: 'Bar' }],
  },
});
```

결과는

- Parent - 0 - Foo
- Parent - 1 - Bar

`in` 대신에 `of`를 구분자로 사용할 수 있다.

```html
<div v-for="item of items"></div>
```

#### 7.2 v-for와 객체

v-for를 이용하여 객체의 속성도 반복하여 렌더링 할 수 있습니다.
`value in object` 형태의 특별한 문법이 필요합니다.

```html
<ul id="v-for-object" class="demo">
  <li v-for="value in object">
    {{ value }}
  </li>
</ul>
```

```js
new Vue({
  el: '#v-for-object',
  data: {
    object: {
      title: 'How to do lists in Vue',
      author: 'Jane Doe',
      publishedAt: '2016-04-10',
    },
  },
});
```

결과는

- How to do lists in Vue
- Jane Doe
- 2016-04-10

두번째 인자로 key를 제공할 수 있습니다.

```html
<div v-for="(value, name) in object">
  {{ name }}: {{ value }}
</div>
```

결과는
title: How to do lists in Vue
author: Jane Doe
publishedAt: 2016-04-10

세번째 인자로는 index도 제공한다.

```html
<div v-for="(value, name, index) in object">
  {{ index }}. {{ name }}: {{ value }}
</div>
```

결과는

0. title: How to do lists in Vue
1. author: Jane Doe
1. publishedAt: 2016-04-10

> 객체를 반복할 때 순서는 Object.keys()의 key 나열 순서에 따라 결정된다. 이 순서는 JavaScript 엔진 구현간에 일관적이지는 않다.

##### 공식문서를 살표보면 더 깊은 내용들이 많지만 사용해보면서 차차 더 알아가도록 하고 오늘과 내일은 기본적인 것에 집중하도록 하겠습니다.

##### 역시나 틀리거나 다른 부분이 있다면 꼭 지적해주시면 감사드리겠습니다!
