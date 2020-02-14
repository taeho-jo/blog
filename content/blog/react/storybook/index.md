---
title: 'storybook 맛보기'
date: '2020-02-12'
category: 'storybook'
---

# Storybook?

- **Storybook**은 UI 구성 요소(컴포넌트)를 개발하기위한 오픈 소스 도구

#### 완전 처음 튜토리얼 수준입니당..참고하실분들만 참고해주세요...

## react에 storybook 설치

- `npx -p @storybook/cli sb init --type react`
- 설치를 하고 나면
  ![](https://images.velog.io/images/jotang/post/9aa6355f-dca4-4f2d-9dae-d9633013ce31/image.png)
- .storybook과 stories라는 폴더가 생성된다.
- package.json의 script에도 자동으로 추가되니 걱정말자
- .storybook 폴더안에는 main.js이라는 파일이 있고 그 안에는

```jsx
module.exports = {
  stories: ['../src/**/*.stories.js'],
  addons: ['@storybook/addon-actions', '@storybook/addon-links'],
};
```

처음에는 src가 아니라 stories라고 되었는데, 그 부분을 src라고 고쳐주어야 src아래의 .stories.js파일을 읽을 수가 있게된다.

- `npm run storybook`으로 실행해준다.
- 아래 모습이 storybook의 모습

![](https://images.velog.io/images/jotang/post/8c48d447-5a93-4b87-ae09-3dfd560b40a4/image.png)

## 실습을 한번 해보자!

##### 이메일과 비밀번호 input & 버튼을 만들어 볼 예정

#### Input

```jsx
import React from 'react';
import InputPage from './InputPage';

export default {
  title: 'Input',
};
//storybook의 카테고리를 만들어준다.

const id = '이메일';
const email = 'email';
const pw = '비밀번호';
const password = 'password';
const style = {
  marginTop: 32,
};

export const EmailInput = () => <InputPage type={email} text={id} />;
export const PasswordInput = () => (
  <InputPage style={style} type={password} text={pw} />
);
// Input이라는 카테고리안에 들어가는 친구들
```

![](https://images.velog.io/images/jotang/post/677c9f6a-b108-4256-a720-a61139a87e3d/image.png)

#### Button

```jsx
import React from 'react';
import { action } from '@storybook/addon-actions';
import LoginButton from './Button';

export default {
  title: 'Button',
};

const message = '로그인';
const actions = {
  clicked: action('clicked'),
};

export const Buttoned = () => <LoginButton text={message} {...actions} />;
```

![](https://images.velog.io/images/jotang/post/a596ff8d-3309-4a5f-91ba-170c33289123/image.png)

- 이렇게 .stories.js 로 파일을 추가하게 되면, storybook에 올라가게 된다.

## 그 외

- 지금은 그냥 storybook에 ui를 올리는 방법 정도만 이해했다.
- action이라는 것과 또 storybook에 올리는 것과 랜더링이 되는 실제 component와의 관계는 어떻게 되는 것인지 사실 아직 이해하지 못하였다.
- addon이라는 것에 더 많은 기능들이 있는데, 그러한 부분에 대해서도 조금더 공부 하여 다시 업로드 할 수 있도록 하겠습니다.

##### storybook과 실제 render되는 component는 별개로 생각해야 할 듯 하다. component를 따로 만들어야 한다는 것이 아니라, 실제로 랜더링을 하는 component는 .stories.js 파일이 아니라, .stories.js의 토대가 되는 component라고 생각 중이다.
