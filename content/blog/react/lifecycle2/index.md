---
title: '자주 사용되는 LifeCycle method'
date: '2019-12-19'
category: 'react'
---

# 자주 사용되는 Lifecycle method

## render()

- 가장 중요하고 여러 method들 중에 유일한 필수 method\
- this.props / this.state에 접근할 수 있다.
- event를 설정하는 곳 외에서는 setState를 사용하여서는 안된다.
- DOM에 접근 하는 것도 해서는 안된다.
- DOM 정보를 가져오고나 state에 변화를 줄 때는 componentDidMount에서 처리해야 한다.

## constructor

- component의 생성자 method로 component를 만들 때 처음으로 실행되고, 초기 state의 값을 정할 수 있다.

## componentDidMount

- 첫 랜더링을 마친 후에 딱 1번 실행한다.
- 이 method안에서 다른 자바스크립트 라이브러리 또는 프레임워크의 함수를 호출하거나, event등록, setTimeout, setInerval, 네트워크 요청 같은 비동기 작업을 처리할 수 있다.

## componentDidUpdate

- 랜더링을 완료한 후에 실행된다.
- update가 끝난 직후에 실행되는 것이기 때문에 DOM관련 처리를 해도 된다.
- preProps / preState를 사용하여서 이전에 가젼던 data에 접근할 수 있다.
- 이전 값과 지금 현재의 값이 다르다면 실행.

```javascript
componentDidUpdate( preProps ){
	if(preProps.todos.length !== this.props.todos.length) {
    	console.log("달라졌어요")
    }
}
```

## componentWillUnmount

- componentDidMount에서 등록한 event,타이머, 직접 생성한 DOM이 있다면 이곳에서 제거 작업을 해야한다.

![image.png](https://images.velog.io/post-images/jotang/a1ba20c0-399d-11ea-92c9-a98a0266d08e/image.png)

> [공식문서 참고](http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)
