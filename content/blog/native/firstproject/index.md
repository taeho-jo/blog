---
title: '나의 첫번째 Native project를 위한 준비!'
date: '2020-02-16'
category: 'react native'
---

# React Native Components

### 1. index.js

```jsx
import { AppRegistry } from 'react-native';
import App from './App';
import { name as appName } from './app.json';

AppRegistry.registerComponent(appName, () => App);
```

- AppRegistry.registerComponent를 사용하여 네이티브 브릿지에서 사용할 모듈을 정한다.
- 첫번째 매개변수인 appName은 모듈의 이름을 지정하고, 두번째 매개변수는 처음으로 렌더링 될 컴포넌트를 지정한다. appName은 자동으로 생성되기때문에 신경쓰지 않아도 괜찮다.

### 2. App.js

```jsx
import React from 'react';
```

- react-native는 React에서 파생되었기 때문에 React를 불러와야한다

```jsx
import {
  SafeAreaView,
  StyleSheet,
  ScrollView,
  View,
  Text,
  StatusBar,
} from 'react-native';
```

- react에서는 html태그를 사용하여 화면을 표시하지만, react-native에서는 native에서 정한 태그만 사용할 수 있다.

#### SafeAreaView

- 아이폰X와 같은 노치디자인에서 상단에 상태바와 하단의 홈 버튼 영역을 제외한 영역에 콘텐츠를 표시할 때 사용한다.
- SafeAreaView를 사용할지 View를 사용할지는 앱의 컨셉에 따라 결정하면 된다.

#### StyleSheet

- 이름 그대로 컴포넌트의 스타일을 적용할 때 사용한다.
- inline으로 작성할 수 있고, StyleSheet을 사용하는 방법도 있다. styled-components도 사용할 수 있다.

#### ScrollView

- 화면 스크롤이 가능한 컴포넌트.
- FlatList, ScrollView, SectionList 등을 스크롤 가능한 컴포넌트로 제공하고 있다.

#### View

- `div`와 비슷한 역할
- View를 이용하여 전체적인 레이아웃을 구성.

#### Text

- 글자를 표시하기 위하여 반드시 사용해야하는 컴포넌트

#### StatusBar

- 화면에 표시되는 컴포넌트는 아니다.
- 상단에 있는 상태 바를 숨기거나, 색을 변경하는데 사용

### 3. 스타일링

#### StyleSheet & inline Style

- `StyleSheet.create` 함수를 사용하여 스타일 객체를 만든 후, 스타일을 적용하고 싶은 부분에 `<View style={styles.container}></View>` 와 같이 스타일 객체를

- 직접 스타일을 객체를 inline으로 넣는 방법
  `<View style={{ backgroundColor: Colors.white }}></View>`

- StyleSheet을 사용하게 되면 반복되는 스타일을 관리할 수 있다.

- inline Style을 사용하면 컴포넌트 함수에서 변수값을 확인하고 동적으로 스타일을 적용할 수 있다.
  `<Text style={{ color: error ? 'red' : 'blue"}}TEST</Text>'

- 카멜표기법을 사용하여야 한다.

### 4. 추가 라이브러리

#### 4.1 TypeScript

- `npm install typescript @types/react @types/react-native --save-dev`
- **typescript** : typescript 라이브러리
- **@types/react** : react의 type이 정의된 파일을 정의하는 파일
- **@types/react-native** : react-native의 type이 정의된 파일을 정의하는 파일

- root폴더에 `tsconfig.json` 파일을 생성하고, 아래 코드를 작성한다.

```jsx
{
  "compilerOptions": {
    "allowJs": true,
    "allowSyntheticDefaultImports": true,
    "esModuleInterop": true,
    "isolatedModules": true,
    "jsx": "react",
    "lib": ["ES6"],
    "moduleResolution": "node",
    "noEmit": true,
    "strict": true,
    "target": "esnext"
  },
  "exclude": [
    "node_modules",
    "babel.config.js",
    "metro.config.js",
    "jest.config.js"
  ]
}
```

- App.js파일을 App.tsx로 파일명을 수정하고, 아래와 같이 작성한다.

```jsx
...
interface Props {}
...
const App = ({}: Props) => {
  ...
};
```

#### 4.2 Styled Components

- StyleSheet과 inline Style말고도 스타일을 적용 시킬 수 있는 방법은 `Styled Components` 이다.
- react와 react-native에 같은 스타일 코드를 적용시킬 수 있다.
- object 형식으로 작성하지 않고, 웹과 동일한 방식으로 작성할 수 있다.
  - **inline & StyleSheet** : `backgroundColor`
  - **styled components** : `bakcground-color`
- props를 이용할 수 있기 때문에, 동적으로 변경하는 스타일을 관리하기 쉽다.

- `npm install --save styled-components`
- `npm install --save-dev @types/styled-components`

```tsx
import React from 'react';
import styled from 'styled-components/native';

interface Props {}

const App = ({  }: Props) => {
  return (
    <>
      <Body>
        <TextStyle>하이루 네이티브</TextStyle>
        <TextStyle>하이루 네이티브</TextStyle>
      </Body>
    </>
  );
};

export default App;

const Body = styled.View`
  background-color: black;
  flex: 1;
  justify-content: center;
  align-items: center;
`;
const TextStyle = styled.Text`
  color: white;
  font-size: 24px;
  font-weight: 900;
`;
```

- styled components를 사용하기 때문에, react-native의 components는 import할 필요가 없어졌다.
- 다만 style-componets를 import하는 것이 아니라, `style-components/native` 로 import하여야한다.

#### 4.3 절대경로로 components 추가하기

- **상대경로** : `'../../../button'`
- **절대경로** : `'~/button'`
- 상대경로를 이용하게 되면 경로가 길어지고 알기 어려워지기 때문에 src폴더를 기준으로 상대경로로 사용하려고 한다.

- `npm install --save-dev babel-plugin-root-import`

- 라이브러리를 설치하고 나면 `babel.config.js`파일이 생성되는데, 아래와 같이 수정한다.

```js
module.exports = {
  presets: ['module:metro-react-native-babel-preset'],
  plugins: [
    [
      'babel-plugin-root-import',
      {
        rootPathPrefix: '~',
        rootPathSuffix: 'src',
      },
    ],
  ],
};
```

- tsconfig.json 파일도 수정한다.

```js
{
  "compilerOptions": {
    ...
    "baseUrl": "./src",
    "paths": {
      "~/*": ["*"]
    }
  },
  "exclude": [
    ....
  ]
}
```

- src폴더를 root폴더에 생성하고 App.tsx파일을 이동시킨다.
- index.js과 \_**\_test\_\_**폴더의 App-test.js파일에서 App을 import하고 잇는 경로를 바꾼다.

```js
import App from './App';
// 위코드에서 아래코드로 수정
import App from '~/App';
```

#### 그 외

- 개발자 메뉴를 사용하기 위해서는 `ctrl+m` 을 이용한다.
