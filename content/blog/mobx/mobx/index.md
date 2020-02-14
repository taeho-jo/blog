---
title: 'mobX 기본개념'
date: '2020-01-22'
category: 'mobx'
---

# mobX

## mobX란?

- Redux와 마찬가지로 react의 **상태 관리 라이브러리**이다.
- mobX 역시 react에 속해있는 라이브러리가 아니기 때문에 일반 javaScript 또는 vue.js와 같은 다른 프레임워크에서도 사용할 수 있다.
- 불변성을 유지를 할 필요성이 없어진다.

## mobX 라이브러리 설치

`npm install mobx mobx-react`

- mobx-react를 설치하는 이유는 react에서 mobX를 사용하기 위함이다.

## mobX의 주요 개념

### 1. observable state (관찰 받고 있는 state)

- mobX가 지켜보고 있는 state를 말한다.(관찰받고 있는 상태!)
- state의 특정 부분이 바뀌게 되면 mobX에서도 정확히 state의 어떠한 부분이 바뀌었는지 알 수 있다.

### 2. computed value (연산된 값)

- event의 발생으로 어떤 연산을 필요로 하는 state의 변화가 생겼을 때, 기존의 상태값을 토대로 새로운 연산 작업을 하게 된다.
- state의 변화가 없다면, 기존의 값을 사용하게 된다.

### 3. action

- Redux의 action과 같은 개념
- 상태에 어떻게 변화를 줄 것인지 정의를 하게 된다.
- 객체의 형태로 만들지 않는다.

### 4. reactions (반응)

- computed는 특정 값이 연산될 때만 처리가 된다면, reactions는 값이 바뀜에 따라 해야 할 일을 정하는 것을 의미한다.

## react와 mobX 함께 사용하기

### decorate

- 해당 컴포넌트의 요소에 mobx 개념을 각각 적용할 수 있도록 해주는 함수.

```jsx
//component를 export할 때, observer로 감싸준다(HOC)
//decorate 함수를 이용하여, 해당 component의 요소에 mobx개념을 각각 적용한다.
decorate(Counter, {
  number: observable,
  increase: action,
  decrease: action,
});
export default observer(Counter);
```

### decorator

- mobX의 정규 문법은 아니지만, babel 플러그인을 통하여 사용할 수 있는 문법
- decorator를 사용하면 decorate 함수를 사용하지 않아도 된다.

`$ yarn add @babel/plugin-proposal-class-properties @babel/plugin-proposal-decorators`

```json
// 설치 후에 npm run eject을 한 후에
// package.json에서 위와 같이 수정하면 적용이 가능하다.
"babel": {
    "presets": [
      "react-app"
    ],
    "plugins": [
        ["@babel/plugin-proposal-decorators", { "legacy": true}],
        ["@babel/plugin-proposal-class-properties", { "loose": true}]
    ]
  }
```

```jsx
@observer
class Counter extends Component {
  @observable number = 0;

  @action
  increase = () => {
    this.number++;
  }

  @action
  decrease = () => {
    this.number--;
  }
```

### store 만들기

- Redux와는 달리 하나의 class에 observable 값과 함수들을 만들어 주면 된다.
  ![image.png](https://images.velog.io/post-images/jotang/363eb7f0-3d41-11ea-aa00-d75a6c670066/image.png)

### store를 component들과 연동하기

- index.js에서 Provider와 store를 먼저 import해준다.
  ![image.png](https://images.velog.io/post-images/jotang/882ef9d0-3d41-11ea-87a6-f5c46265b041/image.png)

- redux의 Provider와 마찬가지로 최상위에 감싸준다.
- Provider에 props로 넣어준다.
  ![image.png](https://images.velog.io/post-images/jotang/a9c2dcb0-3d41-11ea-87a6-f5c46265b041/image.png)

- inject & observer를 import한다.
- 해당 store만 받아와서 사용해도 무방하다.

```jsx
@inject('counter')
//아래에서 사용시
<button onClick={counter.increase}>+1</button>
<button onClick={counter.decrease}>-1</button>
```

- `@inject('store의 이름')` : 해당 스토어를 props로 전달받아서 사용할 수 있게 된다.
  ![image.png](https://images.velog.io/post-images/jotang/14b93780-3d42-11ea-aa00-d75a6c670066/image.png)
- 위의 코드처럼 함수형태로 파라미터를 전달해주면 특정 값만 받아와서 사용할 수 도 있다.

##### 함수형 컴포넌트에서 inject와 observer 적용

![image.png](https://images.velog.io/post-images/jotang/277873d0-3d43-11ea-b4e0-479c062c61f5/image.png)

```jsx
export default BasketItemList;
//1.observer로 감싸준다
export default observer(BasketItemList);
//2.inject로 감싸준다
export default inject()(observer(BasketItemList));
//3.inject에서 store인 market을 받아와서 props로 정의해준다.
export default inject(({ market }) => ({
  items: market.selectedItems,
  total: market.total,
  take: market.take
}))(observer(BasketItemList));
```

###### 주의사항 : BasketItemList를 랜더링하게 될 때, 내부 component인 BasketItem 에도 observer를 구현해 주어야한다, 이 때 성능적으로 최적화가 일어난다고 한다.
