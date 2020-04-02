---
title: 'Koa랑 친해지기'
date: '2020-04-02'
category: 'Koa'
---

# Koa 설치

먼저 새로운 폴더를 생성하고 아래 명령어들을 실행한다.
```
$ npm init
```
````
$ npm install koa
````

# eslint 설정
```
$ yarn install eslint
$ yarn run eslint --init
```

eslint의 경우 실행을 하게 되면 각자 프로젝트에 맞게 선택하여 진행하면된다.

```js
//.eslintrc.js
{
  "env": {
    "browser": true,
    "node": true,
    "es6": true
  },
  "extends": ["eslint:recommended", "prettier"],
  "globals": {
    "Atomics": "readonly",
    "SharedArrayBuffer": "readonly"
  },
  "parserOptions": {
    "ecmaVersion": 2018
  },
  "rules": {
    "no-unused-vars": "warn",
    "no-console": "off"
  },
  "parser": "babel-eslint" 
// Babel을 이용해서 transpiling하기 위해서 설정
}
```
```js
//.prettierc.json
{
  "singleQuote": true,
  "semi": true,
  "useTabs": false,
  "tabWidth": 2,
  "trailingComma": "all",
  "printWidth": 80
}
```

# 서버 띄우기
앞서 알아보았지만 다시 한 번 알아보려고한다.

가장 기본적인 koa로 서버를 띄우는 방법이다.

```js
const Koa = require('koa');
const app = new Koa();

app.use(ctx => {
  ctx.body = "hello world!!!"
});

app.listen(4004);

// node src 로 실행
```
localhost:4004로 접속하면 hello world!!! 가 출력되는 것을 확인 할 수 있다.

# 미들웨어

`ctx => {...}` 이 코드자체가 하나의 미들웨어이다.

이 역시 앞에서 알아보았지만 두 가지의 파라미터를 전달받는다.

- **ctx** : 웹 요청과 응답에 대한 정보
- **next** : 다음 미들웨어를 실행시키는 함수
next를 호출하지 않으면 그 부분에서 요청처리를 완료하고 응답
호출을 한다면 등록된 순서대로 실행된다

**next()** 기본적으로 프로미스를 반환한다. 따라서 작업들이 끝나고 나서 할 작업을 정의할 수 있다 `.then(() => {...})`을 사용해도 되고 `async/await` 역시 사용 가능하다.

```js
const Koa = require('koa');
const app = new Koa();

app.use(async (ctx, next) => {
  console.log(1);
  const started = new Date();
  await next();
  console.log(new Date() - started + 'ms');

});

app.use((ctx, next) => {
  console.log(2);
  next();
});

app.use(ctx => {
  ctx.body = "hello world!!!"
});

app.listen(4000);
```

# nodemon 설치
node server는 코드를 작성하게되면 서버를 종료하였다가 다시 작동시켜야하는 불편함이 있다. 그 부분을 조금 편리하게 사용할 수 있도록 하는 nodemon을 설치한다.

```
$ npm install -g nodemon
```
```
$ nodemon --watch src/ src/index.js
````
src/ 디렉토리에서 코드변화가 감지되면 `자동적으로 재시작`을 하도록 설정하고 src/index.js를 실행시켜주는 명령어

조금 더 편리하게 실행할 수 있도록 package.json의 script에 추가한다.
```json
 (...)
  "scripts":{
    "start": "node src",
    "dev": "nodemon --watch src/ src/index.js"
  }
}
```
이것으로 `npm run dev`또는 `yarn run dev`로 실행할 수 있다.

# koa-router
**koa-router**는 요청이 들어왔을 때, 경로에 따라 다른 작업을 할 수 있게 해준다.
```
$ npm install koa-router
```

### koa-router 기본사용법
```js
const Koa = require('koa');
const Router = require('koa-router');

const app = new Koa();
const router = new Router();

router.get('/', (ctx, next) => {
  ctx.body = 'home';
})

app.use(router.routes());
app.use(rotuer.allowedMethods());

app.listen(4004, () => {
    console.log('실행됨');
});
```
경로에 `'/'`가 들어오면 `home이라는 내용`으로 응답하도록 라우터를 설정하였고,
localhost:4000/ 으로 가면 home이 출력된다.

### 여러개의 라우트, 라우트 파라미터
```js
const Koa = require('koa');
const Router = require('koa-router');

const app = new Koa();
const router = new Router();

router.get('/',(ctx, next) => {
  ctx.body = '홈';
});

router.get('/about', (ctx, next) => {
  ctx.body = '소개';
});

router.get('/about/:name', (ctx, next) => {
  const { name } = ctx.params;
  ctx.body = name + '의 소개';
});

router.get('/post', (ctx, next) => {
  const { id } = ctx.request.query;
  if(id) {
    ctx.body = '포스트 #' + id;
  } else {
    ctx.body = '포스트 아이디가 없습니다';
  }
});

app.use(router.routes());
app.use(router.allowedMethods());


app.listen(4004, () => {
  console.log('실행됨');
});
```
- **/about** : 경로에 `/about` 이 들어오면 `소개라는 내용`으로 응답
- **path => /about/:name** : 이 경로로 들어가면 `동적으로 반응`하게 되고, 뒤의 name이라는 파라미터가진다.
- **query => ?id=10** : 쿼리는 ? 뒤에 붙는다. 쿼리파라미터가 들어 갈 수 있게 해준다.

### 라우트 모듈화
코드의 간결화 그리고 유지보수 측면에서 라우터를 다른 파일에 작성하여 불로오는 방법을 사용한다.

src/api/index.js 을 생성한다.

#### src/api/index.js
```js
const Router = require('koa-router');

const api = new Router();

api.get('/books', (ctx, next) => {
    ctx.body = 'GET ' + ctx.request.path;
});

module.exports = api;
```

#### src/index.js
```js
const Koa = require('koa');
const Router = require('koa-router');

const app = new Koa();
const router = new Router();
const api = require('./api');

router.use('/api', api.routes()); 
// api 라우트를 /api 경로 하위 라우트로 설정

app.use(router.routes()).use(router.allowedMethods());

app.listen(4004, () => {
    console.log('실행됨');
});
```
여러 종류의 api를 만들 때에는 각 종류마다 파일을 분리해서 작성하는 것이 좋다.

api 디렉토리 안에 books라는 디렉토리를 생성하고 그안에 index.js를 생성한다.

#### src/api/books/index.js
```js
const Router = require('koa-router');

const books = new Router();

books.get('/', (ctx, next) => {
    ctx.body = 'GET ' + ctx.request.path;
});

module.exports = books;
```

#### src/api/index.js
index.js에 라우트를 연결해준다.
```js
const Router = require('koa-router');

const api = new Router();
const books = require('./books');

api.use('/books', books.routes());

module.exports = api;
```
localhost:4004/api/books 로 접속하여 확인해보자

#### REST API 메소드
- **GET** : 데이터를 가져올 때 사용
- **POST** : 데이터를 등록할 때, 또는 인증작업을 진행할 때 사용
- **PUT** : 데이터를 업데이트, 교체할 때 사용
- **DELETE** : 데이터를 삭제할 때 사용
- **PATCH** : 데이터의 특정한 필드를 수정할 때 사용

`PUT & PATCH`의 차이는 다음 시간에 알아보도록 하겠다.

라우트에서 메소드에 대한 요청을 할 때는 `.get`, `.post`, `.put`, `.delete`, `patch`를 사용하면 된다.

라우트를 작성할 때, 각 라우트에 해당하는 핸들러는 한눈에 보기 좋도록 따로 작성하는 것이 좋다.

books.controller.js파일을 생성한다.

### src/api/books/books.controller.js
```js
exports.list = (ctx) => {
  ctx.body = 'listed'
};

exports.create = ctx => {
  ctx.body = 'created'
};

exports.delete = ctx => {
  ctx.body ='deleted'
};

exports.replace = ctx => {
  ctx.body = 'replaced'
};

exports.update = ctx => {
  ctx.body = 'updated'
};
```
`exports.변수명 = ...` 으로 내보내고, 불러올 때에는

```js
const 모듈명 = require('파일경로');
```

### src/api/books/index.js
```js
const Router = require('koa-router');

const books = new Router();
const booksCtrl = require('./books.controller');

books.get('/', booksCtrl.list);

books.post('/', booksCtrl.create);

books.delete('/', booksCtrl.delete);

books.put('/', booksCtrl.replace);

books.patch('/', booksCtrl.update);

module.exports = books;
```
포스트맨 프로그램을 이용하여 메소드들을 바꿔가며 통신 진행하며 결과를 확인해보자


#### MongoDB와 연결을 하고, 로그인과 관련된 api를 만드는 것은 내일 업로드 할 수 있도록 하겠습니다.
#### 아직 작성 중입니다.
#### 틀리거나 다른 부분은 가르쳐주시면 감사드립니다

