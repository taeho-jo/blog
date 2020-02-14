---
title: 'class & ES6(arrow function)'
date: '2019-12-01'
category: 'JavaScript'
---

# 1. class

### 1.1 객체 생성자

- 붕어빵의 틀을 갖는 것과 비슷
- 함수를 통해서 새로운 객체를 만들고 그 안에 넣고 싶은 값 혹은 함수들을 구현 할 수 있게 해준다.

```javascript
function Animal(type, name, sound) {
  this.type = type;
  this.name = name;
  this.sound = sound;
  this.say = function() {
    console.log(this.sound);
  };
}
const dog = new Animal('개', '멍멍이', '멍멍');
const cat = new Animal('고양이', '야옹이', '야옹');
dog.say(); // 멍멍
cat.say(); // 야옹
```

- 함수의 이름을 대문자로 시작
- 새로운 객체를 만들 때에는 **`new`** 를 앞에 붙여주어야한다.

### 1.2 class

- object 구조의 객체 틀을 만들고, 비슷한 모양의 객체를 공장처럼 찍을 수 있다.
- 상속이 훨씬 쉽다.
- css의 class와는 전혀 다른 개념
- object와 class는 문법이 비슷
- class에는 **cunstructor**라는 생성자 함수가 필요

```javascript
class Car {
  constructor(name, price) {
    this.name = name;
    this.price = price;
  }
}
//class를 통해 생성된 object를 instance라고 부른다.
//Car는 class의 이름(항상 대문자로 시작)
//class의 실행범위에서 this는 해당 instance를 의미
//name, price 같은 인자는 변경 가능한 상태값이자, class 내부의 어느 곳에서나 사용
//할 수 있는 변수를 '멤버변수'라고 부른다
//'멤버변수'는 this로 접근
```

### 1.3 class(Instance)

- class를 통해서 생성된 객체
- class의 property이름과 method를 갖는 객체
- instance마다 모두 다른 property값을 갖는다.

```javascript
const morning = new Car('Morning', 20000000);
//instance는 new를 붙여서 생성
//cunstructor에 필요한 정보를 인자로 넘겨줌
//instance를 새로운 변수에 저장
```

### 1.4 class(methods)

- method는 함수
- object가 property값으로 갖고 있는 것을 method라 한다.
- class의 method는 object와 똑같은 문법
- 객체는 property마다 `,`(쉼표)로 구분, but class는 아님.

```javascript
class Car {
  constructor(name, price) {
    this.name = name;
    this.price = price;
    this.department = '선릉지점';
  }
  applyDiscount(discount) {
    return this.price * discount;
  }
  changeDepartment(departmentName) {
    this.department = departmentName;
  }
}
const ray = new Car('ray', 2500000);
const socar = new Car('socar', 3000000);
```

```javascript
//총 4개의 method 구현
class MyMath {
  constructor(num1, num2) {
    this.num1 = num1;
    this.num2 = num2;
  }
  add(num1, num2) {
    return this.num1 + this.num2;
  }
  multiply(num1, num2) {
    return this.num1 * this.num2;
  }
  substract(num1, num2) {
    return this.num1 - this.num2;
  }
  getNumber(num1, num2) {
    return [this.num1, this.num2];
  }
}
const math = new MyMath(3, 4);
console.log(math);
console.log(math.add());
console.log(math.multiply());
console.log(math.substract());
console.log(math.getNumber());
```

# 2. [ES6] arrow function

```javascript
//ES5
function getN(userInfo, label) {
  return userInfo[label];
}
//ES6
const getN = (userInfo, label) => {
  userInfo[label];
};
```

```javascript
//ES5
let sayHi = function() {
  alert('Hello');
};
console.log(sayHi());
//ES6
let sayHi = () => {
  alert('Hello');
};
sayHi();
```

```javascript
//ES6
const getName = info => info['name'];
//ES5
function getName(info) {
  return info['name'];
}
```

# 3. [ES6] template literals, string method

### 3.1 template literals

```javascript
//ES5
function getEmail(username) {
  return username + '@gmail.com';
}
//ES6
const getEmail = username => `${username}@gmail.com`;
getEmail('jotang');
```

```javascript
const name = '조개발';
//ES5
const hi = '안녕하세요. 저는 ' + name + ' 입니다.';
//ES6
const hi = `안녕하세요. 저는 ${name} 입니다.`;
```

```javascript
//enter 처리(break line)
//ES5
let detail = '오늘도\n' + '내일도\n' + '화이팅';
//ES6
let detail = `오늘도
내일도
화이팅

코딩..`;
//back tick(``)사용으로 srting을 입력한대로 처리된다
```

### 3.2 string method

- 3가지 method
- startsWith : string의 시작 문자
- endsWith : string의 마지막 문자
- includes : stirng에 포함되어 있는 문자

```javascript
const email = 'jotang3726@gmail.com';
console.log(email.indexOf('8')); //8이 없음으로 -1반환
console.log(email.startsWith('37')); //false
console.log(email.startsWith('jo')); //true
console.log(email.endsWith('com')); //ture
console.log(email.includes('@gmail')); //ture
//특정 문자열의 반복(repeat 함수 사용)
'@'.repeat(10);
```

> 참조 : https://javascript.info/string
