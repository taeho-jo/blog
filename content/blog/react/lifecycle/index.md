---
title: 'Life Cycle'
date: '2019-12-14'
category: 'react'
---

# Lifecycle

![image.png](https://images.velog.io/post-images/jotang/d220eec0-3997-11ea-88d5-a5a384879d74/image.png)

- Lifecycle method는 총 9가지.
- **Will** : 어떠한 작업을 작동하기 전에 실행되는 method
- **Did** : 어떠한 작업이 작동한 후에 실행되는 method

# 큰 흐름을 이해하기

1. **mount**: page에 component가 나타남(DOM이 생성되고 웹 브라우저상에 나타는 것을 마운트(mount) 라고 한다)

   - constructor : component를 새로 만들 때 호출되는 클래스 생성자 method
   - getDerivedStateFromProps: props에 있는 값을 state에 넣을 때 사용하는 method
   - render: UI를 랜더링하는 method(필수!)
   - componentDidMount: component가 웹 브라우저상에 나타난 후에 호출하는 method

2. **update** : component의 정보를 업데이트 하여 리 랜더링

- updata는 props가 바뀔 때 / state가 바뀔 때 / 부모 component가 리랜더링 될 때 / this.forceUpdate로 강제로 랜더링을 할 때 - getDerivedStateFromProps : update가 시작되기 전에도 호출된다. props의 변화에 따라 state 값에도 변화를 주고 싶을 때 사용한다.
  - shouldComponentUpdate : 리랜더링을 할지 말지 결정하는 method. true || false를 반환해야한다.
  - render : component를 랜더링 한다(필수요소)
  - componentDidUpdate : component의 update 작업이 끝난 후 호출되는 method.

3. **unmount** : page에서 component가 사라짐(mount의 반대과정, component를 DOM에서 제거하는 것을 말한다.)

   - componentWillUnmount : component가 웹 브라우저상에서 사라지기 전에 호출하는 method

![image.png](https://images.velog.io/post-images/jotang/e92e0bf0-3999-11ea-a3ea-c3f18171ceb1/image.png)

> [공식문서 참고](http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)
