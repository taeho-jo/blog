---
title: 'Next.js mobX Provider 적용'
date: '2020-01-29'
category: 'next.js'
---

# Next.js & mobX

##### 어제 작성 했던 next.js에 mobX를 적용하기위하여 Provider를 사용하는 부분을 수정하고자 한다. 어제 했던 세팅으로 사용했을 경우에 router를 사용하는 부분에서 에러가 발생 하였다.

`MobX Provider: The set of provided stores has changed. Please avoid changing stores as the change might not propagate to all children`

##### 제일 첫 화면은 랜더링이 잘 되지만 router를 이용하여 page 이동을 하면 위의 에러 코드가 뜬다. provider에 적용한 store가 변경되었다고 나온다. 새로운 페이지로 넘어갈 때, 최상위 페이지인 \_app.js부터 렌더링 된 후 해당되는 페이지로 넘어가게 된다. \_app.js파일이 새로 렌더링 되면 Store를 다시 생성하여 provider에 적용하게 되어있다. Store를 새로 적용하게 되면, 기존에 관리되던 모든 State 상태가 날아가게 되므로, Store를 새롭게 적용하지 못하도록 되어있다.

##### 사실 적용은 했지만, 완벽히 이해하지 못해 설명에 적지 못하는 부분이 많을 것이다. 앞으로 더 개념을 채워서 보충하도록 하겠습니다.

## store 생성

- store는 이전과 같이 생성해준다.

## store들의 RootStore 만들기

```jsx
import { useStaticRendering } from 'mobx-react';

import CounterStore from './counter';
import PlusStore from './plus';

const isServer = typeof window === 'undefined';
useStaticRendering(isServer);

let store = null;

export default function initializeStore(initialData) {
  if (isServer) {
    return {
      counter: new CounterStore(),
      plus: new PlusStore(),
    };
  }
  if (store === null) {
    store = {
      counter: new CounterStore(),
      plus: new PlusStore(),
    };
  }

  return store;
}
```

- useStaticRendering을 import해온다 (**useStaticRendering**에 대해서는 추가로 업로드예정)
- 만들어두었던 store를 import해온다.
- 위 코드와 같이 작성한다.

## \_app.js 설정

```jsx
import React from 'react';
import App, { Container } from 'next/app';
import Head from 'next/head';
import { globalStyles } from '../styles/reset';
import { Provider } from 'mobx-react';

import initializeStore from '../stores/stores';

class MyApp extends App {
  static async getInitialProps(appContext) {
    const mobxStore = initializeStore();
    appContext.ctx.mobxStore = mobxStore;
    const appProps = await App.getInitialProps(appContext);
    return {
      ...appProps,
      initialMobxState: mobxStore,
    };
  }

  constructor(props) {
    super(props);
    const isServer = typeof window === 'undefined';
    this.mobxStore = isServer
      ? props.initialMobxState
      : initializeStore(props.initialMobxState);
  }
  render() {
    const { Component, pageProps } = this.props;
    return (
      <Provider {...this.mobxStore}>
        <Head>
          <meta charSet="utf-8" />
          <style type="text/css">{globalStyles}</style>
          <title>DeusAdventures</title>
        </Head>
        <Container>
          <Component {...pageProps} />
        </Container>
      </Provider>
    );
  }
}

export default MyApp;
```

- 위 코드와 같이 작성한다.

## component에서 사용하기

```jsx
import React, { Component } from 'react';
import { observer, inject } from 'mobx-react';

@inject('counter')
@observer
class Counter extends Component {
  render() {
    const { counter } = this.props;
    return (
      <div>
        <h1>{counter.number}</h1>
        <button onClick={counter.increase}>+1</button>
        <button onClick={counter.decrease}>-1</button>
      </div>
    );
  }
}

export default Counter;
```

- 사용하는 방법은 동일하다.
