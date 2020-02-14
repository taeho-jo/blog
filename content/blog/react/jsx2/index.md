---
title: 'Component ( class & function )'
date: '2019-12-09'
category: 'react'
---

# class형 Component

### function형 componenet의 구조

```jsx
import React from 'react';
import './App.css';

function App() {
  const name = '리액트';
  return <div className="react">{name}</div>;
}
export default App;
```

### class형 component의 구조

```jsx
import React from 'react';

class App extends React.Component {
  render() {
    const name = '리액트';
    return <div className="react">{name}</div>;
  }
}
export default App;
```

- class와 function의 차이점은 class형 component의 경우에 state 및 라이프사이클 기능을 사용할 수 있고, 임의 method를 정의할 수 있다.
- class형 componenet에는 render()함수는 필수로 있어야한다.

## Class & function의 차이점

#### function Component

- 메모리 자원이 class형 보다 덜 사용된다.
- state와 라이프사이클 API의 사용이 안되었지만, Hooks 기능이 도입되면서 해결되었다.
