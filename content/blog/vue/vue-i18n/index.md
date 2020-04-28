---
title: 'Vue-i18n 사용기'
date: '2020-04-27'
category: 'Vue'
---

다국어를 지원하기 위한 i18을 vue에 적용시켜보려고 한다.

### 설치
`npm install vue-i18n`

매우 간단하게 설치를 할 수있다.

### 적용하기
`main.js`파일로 가서 먼저 import를 진행해준다.

```js
import VueI18n from 'vue-i18n`

Vue.use(VueI18n)
```

main.js파일에서 아래와 같이 작성하면 일단 적용은 끝!
```js
const messages = {
  en: {
    message: {
      hello: 'hello world'
    }
  },
  ko: {
    message: {
      hello: '안녕'
    }
  }
}

const i18n = new VueI18n({
  locale: 'ko',
  messages
})

new Vue({
  router,
  store,
  i18n,
  render: h => h(App),
}).mount('#app');
```

### 파일 분리하기
main.js에 번역해야하는 모든 부분을 작성하게 되면 코드가 지저분해지기 때문에, 번역할 파일을 분리해준다.

`locales`폴더를 생성하고 `index.js` 파일을 생성하고 그 안에 작성한다.

```js
// locales/index.js

export const messages = {
  en: {
    message: {
      hello: 'hello world'
    }
  },
  ko: {
    message: {
      hello: '안녕'
    }
  }
}
```

```js
// main.js

import { messages } from './locales';
```
끝 그럼 화면에 출력을 해보도록 한다.

언어를 전환할 수 있도록 버튼을 두 개 만들고, 화면에 출력을 해보도록한다.
```html
<template>
  <div class="home">
    <button @click="$i18n.locale='ko'">한글</button>
    <button @click="$i18n.locale='en'">영어</button>
    <p>메세지</p>
    <p>{{$t('message.hello')}}</p>
  </div>
</template>
```

![](https://images.velog.io/images/jotang/post/f4774f78-fc54-4a9f-8249-b9c73a4cfb21/image.png)
![](https://images.velog.io/images/jotang/post/ef7a917d-f2f6-46bf-802c-f25fbb9c2a53/image.png)

### 날짜 & 통화
vue-i18n은 날짜와 통화표시까지 지원해주고 있다.

날짜는 `dateTimeFormats`를 이용한다.
```js
// locales/index.js 에 추가

export const dateTimeFormats = {
  'en-US': {
    short: {
      year: 'numeric', month: 'short', day: 'numeric'
    },
    long: {
      year: 'numeric', month: 'numeric', day: 'numeric',
      weekday: 'short', hour: 'numeric', minute: 'numeric'
    }
  },
  'ko-KR': {
    short: {
      year: 'numeric', month: 'short', day: 'numeric'
    },
    long: {
      year: 'numeric', month: 'numeric', day: 'numeric',
      weekday: 'short', hour: 'numeric', minute: 'numeric', hour12: true
    }
  }
}
```
locale을 ko에서 ko-KR / en 에서 en-US로 바꿔준다
```js
const i18n = new VueI18n({
  locale: 'ko-KR',
  messages,
  dateTimeFormats,
})
````
```html
<!-- 버튼에 주었던 이벤트의 locale도 변경해준다 -->
<button @click="$i18n.locale='ko-KR'">한글</button>
<button @click="$i18n.locale='en-US'">영어</button>

날짜
<p>{{ $d(new Date(), 'short') }}</p>
<p>{{ $d(new Date(), 'long') }}</p>
```

![](https://images.velog.io/images/jotang/post/e6332ce1-5747-4686-946c-331e0b922244/image.png)
![](https://images.velog.io/images/jotang/post/d6300148-3063-4079-aaae-869369e67407/image.png)


통화 역시 같은 방식으로 적용시키면 되고, `numberFormats`를 이용한다.
```js
// localse/index.js

export const numberFormats = {
  'en-US': {
    currency: {
      style: 'currency', currency: 'USD'
    }
  },
  'ko-KR': {
    currency: {
      style: 'currency', currency: 'KRW', currencyDisplay: 'symbol'
    }
  }
}

// main.js
const i18n = new VueI18n({
  locale: 'ko-KR',
  messages,
  dateTimeFormats,
  numberFormats
})
```
```html
<!-- html 추가 -->
통화
<p>{{ $n(999, 'currency') }}</p>
<p>{{ $n(9999, 'currency') }}</p>
```
![](https://images.velog.io/images/jotang/post/b0901447-612c-4e4d-b2b4-da45167f9368/image.png)
![](https://images.velog.io/images/jotang/post/bb539819-22a0-487c-a105-29b574c6ef94/image.png)

vue-i18n의 아주 간단한 사용법을 알아보았다.
더 많은 속성과 더 많은 기능을 제공해주고있다.
[vue-i18n 사이트](https://kazupon.github.io/vue-i18n/)를 참고하길 바란다.



