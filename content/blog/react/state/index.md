---
title: 'State'
date: '2019-12-13'
category: 'react'
---

# class형의 state

- state란 component 내부에서 바뀔 수 있는 값을 의미한다.
- constructor method를 작성하여서 설정한다.

```jsx
constructor(props){
	super(props)

  	this.state={
    	number: 0,
      	name: ""
    }
}
```

- state는 object의 형태이다.

## state 값의 변화

- state의 값을 변화시킬 때 직접적으로 state를 건드리면 안되고 setState를 이용하여 값을 변화 시켜야한다.

```jsx
this.setState = {
  number: number + 1,
  name: 'jotang',
};
```

## state 비구조화 할당

```jsx
render(){
	const { number, name } = this.state
    return(
      <div>
        <h1>{name}</h1>
      </div>
    )
}
```

## constructor 선언 없이 state 설정

```jsx
import React, { Component } from 'react';

class Counter extends Component {
  state = {
    number: 0,
    fixedNumber: 0,
  };

  render() {
    return <div>return안 내용은 생략합니다.</div>;
  }
}
```

## 비동기적 업데이트

- this.setState를 사용하여 state 값을 업데이트할 때는 상태가 비동기적으로 업데이트 된다.

```jsx
onClick={() => {
	this.setState({ number: number + 1 })
  	this.setState({ number: this.state.number + 1 })
}}
```

- this.state를 두 번 사용을 하지만 버튼을 클릭할 때 숫자는 1씩 더해진다.
- 이에 대한 해결책은 this.setState를 사용할 때 객체 대신에 함수를 인자로 넣어주는 것이다.

```jsx
this.setState((prevState, props) => {
  return {
    //업데이트하고 싶은 내용
  };
});
```

- prevState는 기존 상태이고, props는 현재 지니고 있는 props를 가리킨다. props는 생략가능하다.

## callback함수 이용

- setState를 사용한 후에 특정 작업을 하고 싶을 때는 setState의 두 번째 파라미터로 callback함수를 등록하여 사용할 수 있다.

```jsx
onClick = {() => {
	this.setState(
      {
    	number: number + 1
      },
     () => {
     	console.log('방금 setState가 호출되었습니다.')
        console.log(this.state)
     }
    )
}}
```
