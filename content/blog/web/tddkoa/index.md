---
title: 'TDD & Koa'
date: '2020-03-31'
category: 'web'
---

##### 이전에 리팩토링에 대해서 작성한 글을 많이는 아니지만 보충해보려고 한다.
##### 아직 어려운 내용이 많기 때문에 깊지 않고, 이해한 내용을 바탕으로 작성하다보니 틀린 부분이 있을 수 있습니다.

# TDD(Test Driven Development)
**TDD**란 테스트 주도 개발이다. 

가장 쉽게 설명할 수 있는 방법은, 테스트코드를 작성 한 이후에 실제코드를 작성하고, 테스트를 통과하면 리팩토링을 진행한다. 이후에는 다시 테스트코드 작성 식으로 흘러간다.

![](https://img1.daumcdn.net/thumb/R800x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FYGxd8%2FbtqyceLGkFd%2FPlES5i8xUIkhzWtw36Vdw0%2Fimg.gif)

### 실패
실패하는 테스트 케이스 작성을 먼저한다.
프로젝트 전체의 기능에 대해서 실패 케이스를 만드는 것이 아닌, 가장 먼저 구현할 기능부터 하나씩 테스트 케이스를 작성한다.

### 성공
실패하는 테스트 케이스를 성공할 수 있도록 실제 코드를 작성한다.

실제 코드를 작성함으로써 테스트를 통과시키는 것이다.

### 리팩토링
실제 작성한 코드에서 중복되는 코드, 혹은 더 개선할 부분에 대해서 리팩토링을 진행한다.
코드를 리팩토링 할 때에 항상 테스트를 진행하면서 한다.

리팩토링이 끝났다면, 다음에 구현할 기능에 대한 실패 테스트 케이스를 작성한다.

# TDD의 장점
테스트 케이스는 구현할 기능을 기준으로 작성하게 된다. 따라서 코드를 작성 할 때, 코드의 양이 방대해지지 않고, 코드의 모듈화가 자연스럽게 이루어지면서 개발이 진행되게 된다.

테스트를 먼저 작성하고 구현을 진행하기 때문에 테스트 커버리지가 높아지게 된다.
이로 인해서 리팩토링, 유지보수가 쉬워진다.

에러로 낭비하는 시간을 줄일 수 있다. 테스트 코드를 작성함으로써 구현할 기능에 대한 요구사항을 충족하는지 자연스럽게 확인이 가능하다.

# Mocha
NodeJS에서 가장 많이 사용하는 테스트 프레임워크이다.

### Mocha 설치
```
$ npm install mocha --save-dev
````

### 테스트 코드 작성

테스트 코드를 작성할 폴더를 생성하고, 그 안에 test.js라는 이름이 붙은 파일을 생성한다.

```js
const assert = require('assert');

dexcribe('테스트의 이름', () => {
  it('기능의 설명 or 기능이름', () => {
    ...
    assert.equal(result, "");
  });
  
  // 비동기 처리 가능
  it('기능의 설명 or 기능이름', async () => {
    const result = await ...
    ...
    assert.equal(result, "");
  });
})
```

### assert
assert는 assertion 함수를 쉽게 사용할 수 있도록 node.js에서 제공하는 표준 모듈

### describe
describe는 suite의 테스트 케이스를 생성한다.
```js
describe('text name', () => {

})
```
describe는 중첩이 가능하다. describe()안에 describe()를 추가할 수 있다.

### it
it을 통해서 테스트 코드를 작성한다.
it의 첫번째 파라미터는 테스트 케이스의 이름을 넣어준다.
두번째 파라미터로는 테스트 케이스의 함수를 넣어준다.
```js
it('test case', () => {
  assert.equal(1+1, 2);
});
```

### Hooks
전제조건을 설정해주고, 테스트 후 테스트들을 정리해준다.
```js
describe('hooks 테스트', function () {
    before(function () {
        //모든 테스트가 실행되기 전에 이 블럭이 실행됩니다.
    });

    after(function () {
        //모든 테스트 실행 후에 이 블럭이 실행됩니다.
    });

    beforeEach(function () {
        //각각의 테스트 실행 전에 이 블럭이 실행됩니다.
    });

    afterEach(function () {
        //각각의 테스트 실행 후에 이 블럭이 실행됩니다.
    })

    //테스트 케이스 작성
})
```

### Exclusive test
특정한 테스트 케이스만 테스트하고 싶다면 `.onle()`를 추가한다.
```js
it.only('test case', () => {
  assert.equal(1+1, 2);
});
```

### Inclusive test
.only()와는 반대로 테스트를 무시하라고 전달한다. `.skip()`을 추가한다.
```js
it.skip('test case', () => {
  assert.equal(1+1, 2);
});
```

### Pending test
콜백이 없는 테스트 케이스를 말한다. "누군가가 이 테스트를 작성해야 합니다" 와 같은 케이스가 작성되기를 기다리는 테스트이다.
```js
var assert = require("assert"); 

describe('계산기 테스트', function () {
    describe('더하기 테스트다!', function() {
       it('누군가 이 케이스를 작성해야 합니다.')
    });
})
```

지금까지 TDD와 Mocha에 대해서 굉장히 기본적인 부분만 알아보았다. 계속 공부하고 익숙해지면 또 업로드 하도록 하겠다.

# Koa
아주 기초적인 부분만 오늘 학습하였기 때문에, 설치방법과 기본적인 사용법에 대해서 알아보도록 하겠다.

### Koa란?
대표적인 Node.js의 프레임워크인 Express를 만든 팀이 제작한 웹 프레임워크이다.

Express보다 더 작고, 더 표현적이고, 더 튼튼한 기초가 되는 것을 목표로 제작하였다고 한다.

### Express & Koa
#### Express
**장점**
- 서버를 쉽게 실행, 운영할 수 있다.
- 내장된 라우터로 코드를 쉽게 재사용 할 수 있다.

**단점**
- 수작업으로 해야하는 부분이 많고, 내장된 에러 핸들링이 없어 미들웨어를 잃어버릴 수 있다.
- 다른 프레임워크에 비해 메모리를 많이 차지한다.

#### Koa
**장점**
- 메모리 차지를 적게하고, 표현력이 좋다.
- 미들웨어의 작성이 다른 프레임워크에 비해 쉽다.
- 개발자가 필요한 미들웨어만 구성해서 사용할 수 있다.
- ES6를 지원하고 있어 ES6 제너레이터를 사용할 수 있다.

**단점**
- 아직까지 개발이 진행 중이기 때문에 불안정하다.
- ES6를 사용하기 위해서는 최신 버전의 node.js를 사용해야한다.
- 미들웨어를 직접 작성할 수 있는 부분이 장점이지만, 단점이 될 수 있다.
- 라우터는 다양한 옵션을 다뤄야한다.

### Koa 설치
koa설치를 위해서는 최신버전(7.6.0이상)의 node가 필요하다.

```
$ npm install koa
```

### Koa 실행
```
$ node 파일명.js
```

### Koa의 기본 사용법

#### Hello Koa
`src/index.js`

```js
consg Koa = require('koa')
const app = new Koa();

app.use(ctx => {
  ctx.body = 'Hello Koa';
});

app.listen(4000, () => {
  console.log('안뇽');
})
```

Koa프레임워크를 `require()`를 이용하여 가져온 후, app을 생성한다.

`use()`로 앱이 실행되었을 때의 동작을 지정해 준다.

`listen()`을 통해 4000번 포트로 서버를 동작 시킨다.
콜백으로 서버가 실행된 이후의 동작을 지정해줄 수 있고, 생략도 가능하다.

#### Middleware
Koa는 여러 미들웨어가 연결되어 구성된다. `app.use()`를 통하여 사용할 미들웨어를 등록해줄 수 있다. 
`use()`의 파라미터로 넘겨준 미들웨어 함수는 `ctx`, `next`를 전달 받는다

- **ctx** : 웹 요청과 응답에 대한 정보를 저장

- **next** : 다음 미들웨어를 실행 시키는 함수

next()를 실행하면 다음 미들웨어로 전달되고, 실행하지 않으면 해당 미들웨어에서 요청 처리를 완료하고 응답하게 된다.
미들웨어는 등록된 순서대로 실행된다.
```js
const Koa = require('koa');
const app = new Koa();

app.use((ctx, next) => {
    console.log('middleware 1');
    next();
});

app.use((ctx, next) => {
    console.log('middleware 2');
});

app.use((ctx, next) => {
    console.log('middleware 3');
});

app.listen(4000);
// middleware 1과 2가 순차적으로 실행된다.
```

#### next()
기본적으로 `next()`는 프로미스를 반환한다. 따라서 미들웨어의 실행이 끝난 후 동작을 지정해 줄 수 있다.
```js
const Koa = require('koa');
const app = new Koa();

app.use((ctx, next) => {
    console.log('--- middleware 1 ---');
    ctx.body = 'Hello, World!';
    console.log("middleware 1's body:", ctx.body);
    next()
        .then(() => {
            console.log("Body Message Changed!");
        });
});

app.use((ctx, next) => {
    console.log('--- middleware 2 ---');
    ctx.body = 'Goodbye, World!';
    console.log("middleware 2's body:", ctx.body);
});

app.listen(4000);
```
```
// 실행결과
--- middleware 1 ---
middleware 1's body: Hello, World!
--- middleware 2 ---
middleware 2's body: Goodbye, World!
Body Message Changed!
```

ES7에 추가된 async/await 문법을 사용할 수 있다. (콜백을 없앨 수 있다.)
```js
const Koa = require('koa');
const app = new Koa();

app.use( async (ctx, next) => {
    console.log('--- middleware 1 ---');
    ctx.body = 'Hello, World!';
    console.log("middleware 1's body:", ctx.body);

    await next();
    console.log("Body Message Changed!");
});

app.use((ctx, next) => {
    console.log('--- middleware 2 ---');
    ctx.body = 'Goodbye, World!';
    console.log("middleware 2's body:", ctx.body);
});

app.listen(4000);
```

### nodemon
nodemon은 node서버에 편리함을 가져다 주는 라이브러리이다.

코드를 수정하게 되면 수정된 내용을 적용하기 위해서 node서버를 종료하고 다시 실행해야하는데, **nodemon**는 코드의 수정을 감지하여 자동으로 서버를 재실행시켜준다.

```
$ npm install -g nodemon
```
global로 설치 하지 않아도 된다.

일반적으로 프로젝트를 진행하게 되면, src파일 안에 JavaScript코드를 관리하게 된다.
src폴더 안에 index.js를 생성하고 아래의 명령어를 실행한다.
```
$ nodemon --watch src/ src/index.js
```

index.js파일이 수정되면 서버는 재실행되고, src디렉토리의 모든 변화를 감지하기 때문에src폴더에 새로운 파일을 추가하여도 서버가 재실행된다.

package.json 파일에 script를 추가해서 실행하여도 된다.
```json
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "nodemon --watch src/ src/newBlog.js"
},
```

##### 오늘 학습하게된 TDD, Mocha, Koa에 대해 기본만 알아보고 정리하게 되었다. 앞으로 개발을 해나가면서 더 깊게 공부하여 발전된 내용으로 업로드하도록 하겠습니다.
