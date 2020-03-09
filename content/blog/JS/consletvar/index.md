---
title: 'const & let & var / prototype & class'
date: '2020-03-09'
category: 'JavaScript'
---

# const / let / var

### var는 es6이전, const / let es6 이후

JavaScript에서 변수를 선언하는 방법은 세가지가 있다. 어떤 것들이 있는지 알아보고 그 차이를 알아보려고 한다.

## const

- const는 값이 바뀌지 않는 상수를 선언한다.
- 값의 변경(재할당), 변수의 재선언 둘다 불가능하다.

```js
const a = 123;
console.log(a); // 123

a = 456;
console.log(a);
// Uncaught TypeError: Assignment to constant variable.

const a = 789;
// SyntaxError: Identifier 'a' has already been declared
```

## let

- let은 값의 변경(재할당)은 가능하지만, 변수의 재선언은 불가능하다.

```js
let a = 123;
console.log(a); // 123

a = 456;
console.log(a); // 456

let a = 789;
// SyntaxError: Identifier 'a' has already been declared
```

## var

- var는 값의 변경(재할당), 변수의 재선언 둘 다 가능하다.

```js
var a = 123;
console.log(a); // 123

a = 456;
console.log(a); //456

var a = 'jotang';
console.log(a); // jotang
```

## let & var 의 차이점

차이점을 먼저 알아보기 전에 scope에 대해서 먼저 알아보도록 한다.

### scope

- **변수에 영향을 미치는 범위** 또는 **변수의 유효 범위** 라고 정의할 수 있다.

- `{}(bracket, 브라켓)`으로 감싸진 범위.
- **if / for / function** 의 bracket은 모두 `block-scope`.
- `function-scope`는 block-scope 중에서 function의 블록이 갖는 범위를 말한다.

### const & let / var의 유효범위

- const와 let은 각 block-scope 안에서 유효하다.
- var는 function-scope에서 유효하다.

```js
// const & let
function a() {
  for (let i = 0; i < 3; i++) {
    console.log(i); // 0, 1, 2
  }
  console.log(i); // i is not defined
}

a();

function b() {
  for (var i = 0; i < 3; i++) {
    console.log(i); // 0, 1, 2
  }
  console.log(i); // 3
}
console.log(i); // i is not defined

b();
```

# Prototype

JavaScript는 객체지향 개념을 지원하기 위해 prototype을 사용.

- prototype으로 구현할 수 있는 대표적인 객체지향 개념은 **상속**이다.

### JavaScript에서의 객체 생성.

JavaScript에서 채택하고 있는 자바의 몇 가지 문법 중 대표적인 것은 **new**!
JavaScript는 es6 이전에는 class 키워드가 없었다. 따라서 es6 이전 JavaScript에서는 function으로 정의했다.

```js
function Person(name, age) {
  this.name = name;
  this.age = age;
}

const jotang = new Person('jotang', 32);
console.log(jotang.name); // jotang
console.log(jotang.age); // 32
```

- 객체지향 관점에서 보면 JavaScript에서 function은 자바의 class와 생성자를 합쳐 놓은 개념.
- es6에서 다른 언어와의 이질감을 줄이고, 객체지향을 조금이라도 더 지원하기 위해 class 키워드를 새로 만들게 되었다.

```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}

const jotang = new Person('jotang', 32);
console.log(jotang.name); // jotang
console.log(jotang.age); // 32
```

# Class

es6에서는 class 키워드가 추가돼서 클래스를 정의하여 사용할 수 있게 되었다.

```js
class Car {
  constructor(name) {
    this.name = name;
    this.type = 'Car';
  }
  getName() {
    return this.name;
  }
}

let car = new Car('BMW');
console.log(car.getName()); // BMW
```

- 서브 클라스를 사용할 수 도 있다.

```js
class Suv extends Car {
  constructor(name) {
    super(name);
    this.type = 'Suv';
  }
}

let suv = new SUV('My SUV');
console.log(suv.getName());
```

- 부모 클래스의 생성자를 호출하려면, super 키워드를 사용하면 된다.
- Suv 클래스에서 정의하지 않은 getName() 함수도 동일하게 사용할 수 있다.

##### this에 대해서 더 학습을 진행하려고 하는데, 이해가 되고 정리가 되면 블로깅 하도록 하겠습니다.
