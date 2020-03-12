---
title: 'async & await'
date: '2020-03-12'
category: 'JavaScript'
---

# async function

async function 선언은 AsyncFunction 객체를 반환하는 하나의 비동기 함수를 정의한다.
비동기 함수는 이벤트 루프를 통해 비동기적으로 작동하는 함수로, 암묵적으로 Promise를 사용하여 결과를 반환하게 된다.

```js
async function 함수이름() {}
const 함수이름 = async () => {};
```

```js
// Promise 객체를 return 하는 함수
function func(ms) {
 return new Promise((resolve, reject) => {
  setTimeout(() => {
   resolve(ms)
  })
 })
}

// Promise 객체를 이용해서 비동기 작업을 수행
func(1000).thne((data)) => {
 console.log(data)
}) // 1000

// Promise 객체를 return하는 함수를 await으로 호출하는 방법
async function asyncFunc() {
 const ms = await func(1000)
 console.log(ms)
}
asyncFunc() // 1000

// 즉시 실행함수로 호출
(async function asyncFunc() {
 const ms = await func(1000)
 console.log(ms)
})()
```

- await을 사용하기 위해서, 항상 async 함수 안에서 사용되어야 한다.

#### reject 처리를 위한 try / catch

```js
function func(ms) {
 return new Promise((resolve, reject) => {
  setTimeout(() => {
   reject(new Error("error입니다.")
  })
 })
}

async function asyncFunc() {
 try {
  const ms = func()
  console.log(ms)
 }
 catch(error) {
  console.log(error)
 }
}
asyncFunc() // error 출력
```

#### async function에서 return 되는 값은 Promise.resolve 함수로 감싸서 return 된다.

#### finally() 역시 사용 가능

```js
function func(ms) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(ms);
    });
  });
}

async function asyncFunc() {
  const ms = await func(1000);
  return 'jotang' + ms;
}

async function main() {
  try {
    const name = await asyncFunc();
    console.log(name);
  } catch (err) {
    console.log(err);
  } finally {
    console.log('끝입니다~~');
  }
}
main();
// jotang1000 끝입니다~~ 출력
```

- async function 의 return 값은 Promise.resolve으로 감싸져서 return 되는 것을 알 수 있다.

#### Promise와 async function의 chaining

```js
function func(ms) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(ms);
    });
  });
}

// Promise의 chaining
func(1000)
  .then(() => func(1000))
  .then(() => func(1000))
  .then(() => func(1000))
  .then(() => {
    console.log('4초 후에 실행');
  }); // 4초 후에 실행

// async await
async function asyncFunc() {
  await func(1000);
  await func(1000);
  await func(1000);
  await func(1000);
  console.log('4초 후에 실행');
}
asyncFunc(); // 4초 후에 실행
```

#### Promise.all / Promise.race

- Promise.all과 Promise.race를 async function에서 사용하기 위해 Promise.all / Promise.race의 return 값을 저장해주어 사용한다.

```js
function func(ms) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(ms);
    });
  });
}

// Promise.all
async function all() {
  const pAll = Promise.all([func(1000), func(2000), func(3000)]);
  console.log(pAll);
}
all(); // [1000, 2000, 3000]

// Promise.race
async function race() {
  const pRace = Promise.race([func(1000), func(2000), func(3000)]);
  console.log(pRace);
}
race(); // 1000
```
