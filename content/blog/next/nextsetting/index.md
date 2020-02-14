---
title: 'Next.js setting'
date: '2020-01-28'
category: 'next.js'
---

# Next.js 초기세팅

## css / reset.js / eslint & prettier / mobx & decorator

### next.js 설치하기

- 먼저 next.js를 설치한다.
  `npm create-next-app`

### css 적용

- 이번 project에서는 Material-UI 를 사용하기로 했기에, css를 사용하게되었다.

* 먼저 설치를 진행한다.
  `npm install @zeit/next-css`

- next.config.js 파일을 root(최상단)에 만들고 아래 코드를 추가한다.

```javascript
const withCSS = require('@zeit/next-css');
module.exports = withCSS({});
```

### reset.js

- src 하위 pages 폴더에 \_app.js 파일을 생성한다

```jsx
import React from 'react';
import Head from 'next/head';
import { globalStyles } from '../styles/reset';

export default function MyApp({ Component, pageProps }) {
  return (
    <>
      <Head>
        <meta charSet="utf-8" />
        <style type="text/css">{globalStyles}</style>
      </Head>
      <Component {...pageProps} />
    </>
  );
}
```

- 위 코드를 작성해준 뒤, src 하위 폴더에 styles폴더를 만들고 reset.js파일을 생성한다. 그 후에 globalStyles를 정의해 준다.

```jsx
export const globalStyles = `
html,
body,
div,
span,
object,
iframe,
h1,
h2,
h3,
h4,
h5,
h6,
p,
blockquote,
pre,
abbr,
address,
cite,
code,
del,
dfn,
em,
img,
ins,
kbd,
q,
samp,
small,
strong,
sub,
sup,
var,
b,
i,
dl,
dt,
dd,
ol,
ul,
li,
fieldset,
form,
label,
legend,
table,
caption,
tbody,
tfoot,
thead,
tr,
th,
td,
article,
aside,
canvas,
details,
figcaption,
figure,
footer,
header,
hgroup,
menu,
nav,
section,
summary,
time,
mark,
audio,
video {
  margin: 0;
  padding: 0;
  border: 0;
  outline: 0;
  font-size: 100%;
  vertical-align: baseline;
  background: transparent;
  font-weight: normal;
  letter-spacing: 1px;
}
​ * {
  box-sizing: border-box;
  text-decoration: none;
  list-style: none;
  color: inherit;
}
​ *:focus {
  outline: none;
  border: none;
}
​ body {
  line-height: 1;
}
​ article,
aside,
details,
figcaption,
figure,
footer,
header,
hgroup,
menu,
nav,
section {
  display: block;
}
​ nav ul {
  list-style: none;
}
​ blockquote,
q {
  quotes: none;
}
​ blockquote:before,
blockquote:after,
q:before,
q:after {
  content: "";
  content: none;
}
​ a {
  margin: 0;
  padding: 0;
  font-size: 100%;
  vertical-align: baseline;
  background: transparent;
}
​
/* change colours to suit your needs */
ins {
  background-color: #ff9;
  color: #000;
  text-decoration: none;
}
​
/* change colours to suit your needs */
mark {
  background-color: #ff9;
  color: #000;
  font-style: italic;
  font-weight: bold;
}
​ del {
  text-decoration: line-through;
}
​ abbr[title],
dfn[title] {
  border-bottom: 1px dotted;
  cursor: help;
}
​ table {
  border-collapse: collapse;
  border-spacing: 0;
}
​
/* change border colour to suit your needs */
hr {
  display: block;
  height: 1px;
  border: 0;
  border-top: 1px solid #cccccc;
  margin: 1em 0;
  padding: 0;
}
​ input,
select {
  vertical-align: middle;
  
}
​ textarea {
  resize: none;
}
input:focus {
  outline:none;
  border:0.5px solid #818181;
}
`;
```

- 이것으로 css와 reset.js 를 설정하는 방법은 마치고 eslint와 prettier에 대해 알아보겠다.

### eslint & prettier

- 굉장히 많은 정보를 찾아보고 도전해본 후 성공한 것을 기록하는 것이기에 100%라고는 할 수 없다.
- npm 설치를 먼저한다.

```jsx
npm install -D eslint prettier eslint-plugin-prettier eslint-config-prettier eslint-plugin-import eslint-plugin-react eslint-plugin-react-hooks @typescript-eslint/parser @typescript-eslint/eslint-plugin
```

- root(최상단)에 .eslintrc 파일을 생성하고, 아래 코드를 작성한다.

```jsx
{
  "env": {
    "browser": true,
    "jest": true,
    "es6": true
  },
  "plugins": ["import"],
  "extends": ["plugin:prettier/recommended"],
  "parserOptions": {
    "ecmaVersion": 2019,
    "sourceType": "module"
  },
  "rules": {
    "no-console": "warn",
    "no-eval": "error",
    "import/first": "error"
  },
  // "parser": "babel-eslint"
}
```

- 이것을 했는데도 오류가 난다면 `npm install babel-eslint --save-dev` 을 설치하고 위 코드 마지막 주석 처리 되어있는 `"parser": "babel-eslint"`를 추가해준다.
- vscode 설정에서 아래 코드를 작성한다(**다를 수 있음에 유의하세요**)

```jsx
{
  "window.zoomLevel": 0,
  "emmet.includeLanguages": {
    "erb": "html",
    "ruby": "html"
  },
  "open-in-browser.default": "Chrome",
  "workbench.colorTheme": "Atom One Dark",
  "workbench.activityBar.visible": true,
  "terminal.integrated.shell.osx": "/bin/zsh",
  "editor.tabSize": 2,
  "editor.detectIndentation": false,
  "javascript.format.enable": false,
  "editor.formatOnSave": true,
  "[javascript]": {
    "editor.formatOnSave": true
  },
  "prettier.disableLanguages": [
    "js"
  ],
  "eslint.alwaysShowStatus": true,
  "files.autoSave": "onFocusChange",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "javascript.updateImportsOnFileMove.enabled": "never",
  "liveServer.settings.donotVerifyTags": true,
  "terminal.integrated.shell.windows": "C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe",
  "javascript.implicitProjectConfig.experimentalDecorators": true
}
```

- 이것으로 eslint & prettier 끝

### mobX / mobx-react / decorator

- mobX의 npm 설치는 이전과 같다.
  `npm install mobx mobx-react`

- decorator의 npm 설치 명령어
  `npm install --save-dev @babel/plugin-proposal-class-properties @babel/plugin-proposal-decorators`

- decorator는 설치 후에 root에 `.babelrc` 파일을 생성하고 아래 코드를 작성한다.

```jsx
{
  "presets": ["next/babel"],
  "plugins": [
    ["@babel/plugin-proposal-decorators", { "legacy": true }],
    ["@babel/plugin-proposal-class-properties", { "loose": true }]
  ]
}
```

- 그래도 빨간줄이 뜬다면 vscode의 experimentaldecorators 설정을 바꾸어주면된다.
- 윈도우 사용자라면 `ctrl + ,` (컨트롤+쉼표) / 맥이라면 커맨드+쉼표 를 한 후에 experimentaldecorators를 검색하여 설정을 해지하면된다.
- mobX 설정은 \_app.js에서 하면된다.

```jsx
import { Provider } from 'mobx-react';
import CounterStore from '../stores/counter';

export default function MyApp({ Component, pageProps }) {
  const counter = new CounterStore();
  return (
    <>
      <Provider counter={counter}>
        <Head>
          <meta charSet="utf-8" />
          <style type="text/css">{globalStyles}</style>
        </Head>
        <Component {...pageProps} />
      </Provider>
    </>
  );
}
```

- src폴더 안에 stores라는 폴더를 생성하고 store를 만든다.(확인을 위해counter store를 만들었음)
- mobx-react의 Provider를 import하고 전체를 감싼다.
- `counter = new CounterStore()` 라고 정의한 후에 Provider에 props로 전달하여 사용한다.

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

- inject를 이용하여 counter를 불러오고 props로 전달하여 사용한다.

```jsx
// **** 함수형태로 파라미터를 전달해주면 특정 값만 받아올 수 있음.
@inject(stores => ({
  number: stores.counter.number,
  increase: stores.counter.increase,
  decrease: stores.counter.decrease,
}))
@observer
class Counter extends Component {
  render() {
    const { number, increase, decrease } = this.props;
    return (
      <div>
        <h1>{number}</h1>
        <button onClick={increase}>+1</button>
        <button onClick={decrease}>-1</button>
      </div>
    );
  }
}
```
