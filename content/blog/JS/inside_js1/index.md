---
title: 'Inside JavaScript 파헤치기_첫번째'
date: '2020-04-28'
category: 'JavaScript'
---

인사이드 자바스크립트를 읽고 하나하나 정리를 시작해보려고한다.
이 책은 ES5기준으로 쓰여져있다.

첫번째인 오늘은 `JavaScript의 기본 개요`, `JavaScript의 데이터 타입과 연산자`에 대해 파헤쳐보도록 하려고한다.

# Chapter01_JS의 기본 개요
## JS 핵심 개념

### 1. 객체
JavaScript는 `거의 모든 것이 객체`.

- 기본 데이터 타입인 `string`, `number`, `boolean`

- `null`, `undefined`

다섯가지를 제외한 나머지는 모두 객체이다.

하지만, 기본 데이터 타입 3가지는 객체처럼 다룰 수 있다.

### 2. 함수
함수 역시 객체로 취급된다. 일반적인 객체보다 조금 더 많은 기능이 있는 객체이다.
`함수는 일급 객체`

### 3. 프로토타입
모든 객체는 숨겨진 링크인 프로토타입을 가지게 된다.

링크는 `해당 객체를 생성한 생성자의 프로토타입 객체를 가리킨다`.
ECMAScript에서는 이 링크를 `[[Prototype]]`이라고 표현한다.

### 4. 실행 컨텍스트와 클로져
JavaScript는 자신만의 독특한 과정으로 실행 컨텍스트를 만들고 그 안에서 실행된다.

실행 컨텍스트는 자신만의 scope를 가지게 된다. 이 과정에서 클로저를 구현할 수 있다.

### 5. 객체지향 프로그래밍 & 함수형 프로그래밍
- **객체지향 프로그래밍**
프로토타입 체인과 클로저로 객체지향 프로그래밍에서 제시하는 상속, 캡슐화, 정보 은닉 등을 소화할 수 있다.

- **함수형 프로그래밍**
함수형 프로그래밍은 높은 수준의 모듈화를 가능하게 하는 매우 효율적인 프로그래밍 방법이다.
일급 객체로서의 함수특성과 클로저를 활용하여 이를 가능하게 하지만, 이러한 이유로 가독성을 떨어뜨리기도 한다.

# Chapter02_JS 데이터타입 & 연산자
기본적으로 JavaScript는 기본 타입과 참조 타입으로 나뉜다.
![](https://images.velog.io/images/jotang/post/cf34bbe6-a3c2-4bfc-9db8-2b0ce9ebf46d/image.png)

## JS의 기본 타입
`string`, `number`, `boolean`, `null`, `undefined`

기본 타입의 특징은 그 자체가 하나의 값을 나타낸다는 것.

JavaScript는 변수에 형태의 데이터를 저장하느냐에 따라 해당 변수의 타입이 결정된다. 따라서 어떠한 데이터라도 변수에 저장할 수 있다.

```js
//number
const num = 10
const floatNum = 0.4

//string
const singleQuote = 'jotang'
const dobuleQuote = "jotang"

//boolean
const yesOrNo = true

//undefined
let empty;

//null
const nullType = null

console.log(
  typeof num,
  typeof floatNum,
  typeof singleQuote,
  typeof dobuleQuote,
  typeof yesOrNo,
  typeof empty,
  typeof nullType
)
```
```
'number' 'number' 'string' 'string' 'boolean' 'undefined' 'object'
```

### 2-1. Number
다른 언어와는 달리 JavaScript는 하나의 숫자형만 존재한다.
모든 숫자를 64비트 부동 소수점 형태로 저장하기 때문

JavaScript에서는 정수형이 따로 없다. 따라서 모든 숫자를 실수로 처리하기 때 나눗셈을 연산할 때에 주의해야한다.

정수 부분만을 구하고 싶다면 `Math.floor()`, `Math.ceil()`등의 메서드를 사용해야 한다.

### 2-2. String
한 번 정의된 문자열은 변하지 않는다는 특징이 있다. 읽기만 가능하며 수정이 불가능하다.
```js
var name = 'jotang';
console.log(name[0], name[1], name[2], name[3], name[4], name[5])

name[0] = 'JJ'
console.log(name)
```
```
'j' 'o' 't' 'a' 'n' 'g'
TypeError: Cannot assign to read only property '0' of string 'jotang'
```

### 2-3. Boolean
true와 false 값을 가진다

### 2-4. undefined & null
먼저 두 타입 모두 `값이 비었음`을 나타낸다.

- **undefined**
  - JavaScript에서 기본적으로 값이 할당되지 않은 변수.
  
  - undefined 타입의 변수는 변수 자체의 값 또한 undefined.
  
  - undefined는 타입이며, 값을 나타낸다.
  
- **null**
  - null 타입 변수의 경우는 명시적으로 값이 비어있음을 나타내는 데 사용.
  - null은 typeof의 결과가 object이다.
  
  - null 타입을 확인하기 위해서는 typeof연산자 대신, `일치 연산자(===)을 사용하여 변수의 값을 직접 확인`해야 한다.
  

## JS의 참조 타입(객체 타입)
배열, 함수, 정규표현식 등 기본 타입을 제외한 모든 값은 객체로 표현된다.

- **이름(key): 값(value)**
- 참조타입인 객체는 여러 개의 프로퍼티들을 포함할 수 있고, 기본 타입의 값을 포함하거나, 다른 객체를 가리킬 수 있다.
- 객체의 프로퍼티는 함수로 포함할 수 있고, JavaScript에서는 이러한 프로퍼티를 메서드라고 부른다.

### 객체 생성
3가지의 방법이 존재한다.
- **Object() 객체 생성자 함수**를 이용

- **객체 리터럴**을 이용

- **생성자 함수**를 이용

### 1. Object() 생성자 함수
빈 객체 생성 후, 프로퍼티를 추가
```js
const person = new Object()
person.name = 'jotang'
person.age = 32
person.gender = 'male'

console.log(typeof person)
console.log(person)
```
```
'object'
{ name: 'jotang', age: 32, gender: 'male' }
```

### 2. 객체 리터럴
리터럴의 의미는 표기법이라고 생각하면 된다.
따라서 객체 리터럴이란 객체를 생성하는 표기법을 의미한다.

key 값으로는 string이나 number가 올 수 있다.
value값으로는 어떠한 표현식도 올 수 있고, value가 함수일 경우 메서드라고 부르게 된다.

```js
const person = {
  name: 'jotang',
  age: 32,
  gender: 'male'
}
```
console.log의 결과는 Object() 객체 생성자 함수의 결과와 같다.

### 3. 생성자 함수
생성자 함수를 이용하여 객체를 생성할 수 있는데, 생성자 함수는 다음 장에서 자세히 알아보도록 하겠다.

### 객체 프로퍼티 읽기/쓰기/갱신
객체의 value에 접근방법은 두 가지가 있다.

- 대괄호 `[]`

- 마침표 `.`

```js
let person = {
  name: 'jotang',
  age: 32,
  gender: 'male'
}

// 읽기
console.log(person.name) // jotang
console.log(person['age']) // 32
console.log(person['gender']) // male

// 갱신
person.age = 20
console.log(person.age) // 20

// 객체 프로퍼티 동적 생성
person.home = 'busan'
console.log(person)
// { name: 'jotang', age: 20, gender: 'male', home: 'busan' }
```

대괄호로 접근해야 하는 경우가 있다.
```js
person['full-name'] = 'JoTaeHo'
console.log(person['full-name']) // JoTaeHo
console.log(person.full-name) // NaN
```

접근하려는 프로퍼티가 표현시이거나 예약어 일 경우에는 대괄호로 접근해야한다.
그리고 대괄호로 접근하기 위해서는 string으로 만들어야한다. `''`필수!!

### for in 문과 객체 프로퍼티 출력
for in 문을 통해서 객체의 모든 프로퍼티의 이름과 값을 출력할 수 있다.
```js
let prop;
for (prop in person) {
  console.log(prop)
  console.log("d",person[prop])
}
```
```
'name' 'age' 'gender' 'home'
'jotang' 20 'male' 'busan'
```

위의 예제에서 확인할 수 있듯, for in 문이 작동하면서 prop에 객체의 key값이 하나씩 할당되는 것을 알 수 있다. prop에 할당된 key값을 이용해서 person[prop]와 같이 대괄호 표기법으로 value값을 출력한다.

### 객체 프로퍼티 삭제
`delete` 연산자를 이용하여 삭제할 수 있다.
객체의 프로퍼티를 삭제할 뿐, 객체 자체를 삭제할 수는 없다.
```js
delete person.age
console.log(person)
// { name: 'jotang', gender: 'male', home: 'busan' }
```

오늘은 JavaScript의 기본 개요와 데이터 타입에서 기본타입 그리고 참조타입 중 객체에 대해서 알아보았다.

다음 시간에는 계속해서 참조 타입의 특성과, 프로토타입, 배열, 그리고 연산자까지 알아보도록 하겠다.
