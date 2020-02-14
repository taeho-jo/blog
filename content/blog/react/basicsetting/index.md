---
title: 'Project 전 React 초기세팅'
date: '2019-12-23'
category: 'react'
---

# Project 전 React 초기세팅

### CRA 설치 (create-react-app)

- 제일 처음으로 cra를 설치한다.

```
npm create-react-app 원하는폴더명
```

- CRA를 설치하고 필요하지 않는 파일들을 정리해준다.

### Sass 설치

- Sass를 설치한다.

```
npm install node-sass --save
```

- 설치를 함으로써 sass를 사용 할 수 있게 된다.

### Router 설치

- 페이지 이동을 위한 Router를 설치한다.

```
npm install react-router-dom --save
```

- 설치 이후에 Router.js파일을 생성한다

```javascript
import React from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import Home from './Pages/Home';
import Signup from './Pages/Signup';
class Routes extends React.Component {
  render() {
    return (
      <Router>
        <Switch>
          <Route exact path="/" component={Home} />
          <Route exact path="/signup" component={Signup} />
        </Switch>
      </Router>
    );
  }
}
export default Routes;
```

- index.js 파일 내에 render함수를 바꿔준다.

```
ReactDOM.render(<Routes />, document.getElementById('root'));
```

### ESLint & Prettier 연동하기

- ESLint란 기본적으로 코드를 검사해서 잘못된 부분들을 짚어주며, Prettier와 연동하여 함께 사용하면 ESLint 규칙에 맞게 코드를 정리해준다.
- ESLint와 Prettier는 VS Code에서 직접 설치해준다.
- VS Code에서만 설치하는 것이아니라

```
npm install prettier eslint-config-prettier eslint-plugin-prettier -D
```

를 통해서 설치해준다.

- 현제 프로젝트 폴더 최상위(root)에서 `.eslintrc.json` 파일을 추가한 후에

```
{
	"extends": ["react-app", "plugin:prettier/recommended"]
}
```

위 내용을 적고 ESLint와 Prettier를 연결해준다.

- VS Code의 setting에 들어가서 우측상단을 클릭하고 원래 있던 객체 파일안에 아래의 내용을 추가해준다.

```
"editor.formatOnSave": true,
"[javascript]": {
  "editor.formatOnSave": false
},
"eslint.autoFixOnSave": true,
"eslint.alwaysShowStatus": true,
"prettier.disableLanguages": ["js"],
"files.autoSave": "onFocusChange"
```

중복된 내용은 합쳐주도록한다. Airbnb에서 규정한 규칙대로 자동 정리되게 된다.

### Project시 주의사항

- 폴더명에 대/소문자 사용, 변수에 \_사용할지 camelCase를 사용할지 미리 정한다.
- git은 대/소문자를 구분하지 못하기 때문에 왠만해서는 폴더명은 바꾸지 않도록한다.

* 불가피하게 변경하게 되었을 때는

```
git mv --force Index.js index.js
```

git 명령을 통해서 바꿔주도록한다.
