---
title: 'Props'
date: '2019-12-12'
category: 'react'
---

# Props

- props는 properties의 줄인 표현으로 component 속성을 설정할 때 사용하는 요소이다.
- props는 부모 component에서 설정할 수 있다.

```jsx
import React from 'react';

const MyComponent = props => {
  return <div>hello, my name is {props.name}</div>;
};
export default MyComponent;
```

```jsx
//부모component에서 props설정
import React from 'react';
import MyComponent from './MyComponent';

const App = () => {
  return <MyComponent name="React" />;
};
export default App;
```

## props 비구조화 할당

```jsx
import React from 'react';

const MyComponent = props => {
  const { name } = props;
  return <div>hi~ my name is {name}.</div>;
};
export default MyComponent;
```

- 비구조화 할당을 통해서 더 짧은 코드를 사용할 수 있다.
- 객체에서 값을 추출하는 문법을 비구조화 할당(destructuring assigmment)이라고 부른다.

# class형에서 Props 사용

- render() 함수에서 this.props를 사용하면 된다.

```jsx
//class형의 props
import React from 'react';

class MyComponent extends React.Component {
  render() {
    const { name } = this.props;
    return <div>hi~ my name is {name}.</div>;
  }
}
export default MyComponent;
```
