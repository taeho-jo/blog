---
title: 'next-i18next ( 언어전환 )'
date: '2020-01-31'
category: 'next.js'
---

# next-i18next 적용

- next-i18next는 언어를 전화해주는 라이브러리로, next.js에 적용할 수 있다.
- 여러 라이브러리가 있지만, SSR때문에 next-i18next를 선택하게 되었다.

# npm 설치

### 라이브러리 사용을 위하여 npm 설치를 해야한다.

- `npm install next-i18next`
- 위의 npm만 설치했을 때 오류가 난다면 아래의 npm도 설치하기 바란다.
- `npm install react-i18next i18next i18next-xhr-backend i18next-browser-languagedetector --save`

# i18n.js

### src폴더에 i18.js파일을 생성한다.

```jsx
const NextI18Next = require('next-i18next').default;

module.exports = new NextI18Next({
  defaultLanguage: ['en'],
  otherLanguages: ['ko', 'en'],
});
```

- 기본 default 언어를 en으로 설정하고, otherLanguages에서 언어들을 추가한다.

### public 폴더에 언어파일을 json형태로 생성한다.

![image.png](https://images.velog.io/post-images/jotang/578f6af0-44ce-11ea-a56b-abfb5f791c5e/image.png)

- public -> static -> locales -> en / ko / chi / de 등등

```json
//ko폴더의 common.json파일
{
  "hi": "안녕",
  "home": "홈화면",
  "bye": "잘가",
  "a": "안뇽 세계",
  "b": "바이바이 세계",
  "c": "넥스트제이에스",
  "d": "하다보면 다되요",
  "e": "대체 왜왜왜"
}
```

# \_app.js 적용 & express

### \_app.js파일에 아래 코드 작성하기

- appWithTranslation을 import해온다

`import { appWithTranslation } from "../i18n";`

- HOC를 이용하여 component를 감싸준다.
  `export default appWithTranslation(MyApp);`

### npm i express

- `npm install express` 를 설치한다.
- root에 server.js 파일을 생성하고 아래 코드를 작성한다.

```jsx
const express = require('express');
const next = require('next');
const nextI18NextMiddleware = require('next-i18next/middleware').default;

const nextI18next = require('./src/i18n');

const port = process.env.PORT || 3000;
const app = next({ dev: process.env.NODE_ENV !== 'production' });
const handle = app.getRequestHandler();

(async () => {
  await app.prepare();
  const server = express();

  await nextI18next.initPromise;
  server.use(nextI18NextMiddleware(nextI18next));

  server.get('*', (req, res) => handle(req, res));

  await server.listen(port);
  console.log(`> Ready on http://localhost:${port}`); // eslint-disable-line no-console
})();
```

### package.json

- 마지막을 package.json 파일로 가서 scripts를 update해준다.

```jsx
{
  	"scripts": {
   	 "dev": "node server.js",
  	  "build": "next build",
   	 "start": "NODE_ENV=production node server.js"
 	 }
}
```

- 준비 끝! 이제 적용을 해볼 시간

# componentsd에서 사용해 보자

- i18n, Link, withTranslation을 import해온다.
- t는 함수이다. t를 통해서 언어의 전환이 가능해진다

```jsx
import PropTypes from 'prop-types';
// 함수로 type을 정의
Home.propTypes = {
  t: PropTypes.func.isRequired,
};
```

- t를 props로 받아와서 사용한다.
- `t{('hi')}` --> common.js에 작성한 hi부분의 내용이 랜더링된다.

```jsx
import React from 'react';
import styled from 'styled-components';
import PropTypes from 'prop-types';
import { i18n, Link, withTranslation } from '../i18n';

const Home = ({ t }) => {
  return (
    <>
      <Link href="/counter">
        <Div>{t('hi')}</Div>
      </Link>
      <Div>{t('home')}</Div>
      <Button onClick={() => i18n.changeLanguage('en')}>EN</Button>
      <Button onClick={() => i18n.changeLanguage('ko')}>KO</Button>
    </>
  );
};

Home.getInitialProps = async () => ({
  namespacesRequired: ['common'],
});
// common.json를 받아온다.

Home.propTypes = {
  t: PropTypes.func.isRequired,
};

export default withTranslation('common')(Home);
// withTranslation으로 Home을 감싸주고, public에 작성해둔 common을 받아온다.

const Div = styled.div`
  width: 100%;
  height: 100%;
  margin-top: 10px;
  color: white;
  font-size: 20px;
  background: #273c75;
`;
const Button = styled.button`
  background: red;
  color: white;
`;
```

# 자동으로 국가코드를 받아온다.

- next-i18next 라이브러리의 기능으로 국가코들 받아와서 자동적으로 해당 언어의 파일을 기본으로 설정하게된다.

![image.png](https://images.velog.io/post-images/jotang/73e088d0-44d1-11ea-80f1-17b58d05445f/image.png)

- localStorage에 저장된다.

- 언어를 추가 하고 싶을 때에는 i18n.js파일의 otherLanguages: ["ko", "en"]에 추가를 한다.

```jsx
otherLanguages: ['ko', 'en', 'chi'];
```

- 그리고 public -> static -> locales 다음 폴더로 chi를 추가하고 json파일을 생성하면 된다.
