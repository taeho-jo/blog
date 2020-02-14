---
title: 'Next.js Redux'
date: '2020-01-16'
category: 'redux'
---

# Next.js Redux

### 초기 설정

1. src 폴더에 새 폴더를 생성한다. (본인은 reducers라고 하였다)

![image.png](https://images.velog.io/post-images/jotang/f813a560-37ca-11ea-bb09-11c18ce8f4d2/image.png)

2. reducers 폴더안에 rootReducer를 위한 index.js파일을 생성하고, action/reducer를 생성할 파일 들을 만든다.

```jsx
import { combineReducers } from 'redux';
import ChangeLanguage from './ChangeLanguage';

const rootReducer = combineReducers({
  ChangeLanguage,
});

export default rootReducer;
```

- combineReducers를 import하고 combine할 reducer 파일들을 import 한 후 rootReducer에 추가한다.

3. CRA에서는 index.js에 Provider 했지만 Next.js에서는 \_app.js에 Provider를 한다.

![image.png](https://images.velog.io/post-images/jotang/0a496660-37cc-11ea-8da2-011fbbd980d3/image.png)

- Provider로 전체를 감싸고, store를 생성한다. (Provider아래에서 중앙통제된다 / store = state + action + reducer)

- Provider는 store를 필요로 하는데 JustSell에는 store를 넣어줄 곳이 없다. 그 부분을 next-redux-wrapper가 해결해주게 된다.

- 하단에서 기존 component인 JustSell을 withRedux으로 감싸게 되고, 기존 component의 기능을 확장하게 된다.(props로 store를 넣어주는 역할을 한다고 보면된다)(**High Order Components**) 그리고 나서 store를 어떻게 넣어줄지 정의하게 된다.

- `const store = createStore(reducer, initialState, composeWithDevTools())`
  이 부분은 정확한 설명을 듣지 못했다. 단지 외우라고..모든 project에서 쓴다며...
  여튼 이 부분이 store를 커스터마이징 하는 부분이다.

- 필요한 npm 설치 목록 \* `npm install --save redux react-redux redux-devtools-extension`
  - `npm install next-redux-wrapper --save`
- redux-devtools : 이 툴을 통해 state의 상태변화를 추적한다거나, time travel 디버깅이 가능하다.

### 언어 전환에 도전

![image.png](https://images.velog.io/post-images/jotang/902d3060-37cf-11ea-bb09-11c18ce8f4d2/image.png)

- 먼저 button이 있는 component로 와서 connect를 import한다.(useSelector/useDispatch 등은 조금 더 이해가되면 공부해서 사용할 예정입니다.)

- 역시 기존 component인 Language를 connect로 감싸고, mapStateToProps와 mapDispatchToProps를 통해서 redux와 연결을 한다.

- mapStateToProps는 첫 번째 인자로 필수로 들어가야하는데, 이 파일에서는 필요하지 않아 null로 작성

- mapDispatchToProps는 필요한 action 함수를 바로 호출하고, 상단에서 import를 한 후에 인자로 넣어주어 사용한다.

![image.png](https://images.velog.io/post-images/jotang/d5a3ef30-37cf-11ea-8da2-011fbbd980d3/image.png)

![image.png](https://images.velog.io/post-images/jotang/f316d820-37cf-11ea-bb09-11c18ce8f4d2/image.png)

![image.png](https://images.velog.io/post-images/jotang/02eaa9c0-37d0-11ea-bb09-11c18ce8f4d2/image.png)

- connect를 이용한다. mapStateToProps를 받아와서 인자로 넣어주어 사용한다.

![image.png](https://images.velog.io/post-images/jotang/59b2b270-37d0-11ea-bb09-11c18ce8f4d2/image.png)

![image.png](https://images.velog.io/post-images/jotang/7c61a240-37d0-11ea-8da2-011fbbd980d3/image.png)

- 아주 잘 바뀌게 된다!
