---
title: 'Inside JavaScript 파헤치기_두번째'
date: '2020-05-10'
category: 'JavaScript'
---

지난 시간에 이어서 Inside_JavaScript를 파헤쳐보려고 한다
# Chapter02_JS 데이터타입 & 연산자
## JS의 참조 타입(객체 타입)
### 참조 타입의 특성
자바스크립트에서는 기본 타입 number, string, boolean, null, undefined 를 제외한 모든 값은 객체이다. 배열 / 함수 역시 객체로 취급한다.

이러한 객체를 자바스크립트에서 **참조 타입**이라고 부른다.

객체의 모든 연산이 실제 값이 아닌 참조값으로 처리되기 때문이다.
```js
const objA = {
  name: 'jotang',
  age: 32
}
const objB = objA

objB.age = 20

console.log(objA) // { name: 'jotang', age: 20 }
console.log(objB) // { name: 'jotang', age: 20 }
```

여기서 objA는 객체 자체를 저장하고 있는 것이 아니라 생성된 객체를 가리키는 참조값을 저장하고 있다.
objB 역시 objA가 가리키는 참조값과 동일한 참조값을 바라본다.


```js
const objA = {
  name: 'jotang',
  age: 32
}
const objB = objA
const objC = {...objA}

objB.age = 20

console.log(objA) // { name: 'jotang', age: 20 }
console.log(objB) // { name: 'jotang', age: 20 }
console.log(objC) // { name: 'jotang', age: 32 }
```


### 객체비교
```js
const objA = {
  name: 'jotang',
  age: 32
}
const objB = {
  name: 'jotang',
  age: 32
}

const objC = objB

console.log(objA === objB) // false
console.log(objB === objC) // ture
```
두 객체를 비교할 때도 객체의 참조값으로 비교를 한다.
objA와 objB는 서로 다른 참조값을 가지고있다.

### 참조에 의한 함수 호출 방식
```js
let a = 100
let b = { value: 100 }

const change = (num, obj) => {
  num = 200
  obj.value = 200
  
  console.log(num) // 200
  console.log(obj) // { value: 200 }
}

change(a, b)

console.log(a) // 100
console.log(b) // { value: 200 }
```

기본 타입의 경우에는 **값에 의한 호출 방식**, 즉 함수를 호출할 때 인자로 기본 타입의 값을 넘기게 될 경우, 호출된 함수의 매개변수로 **복사된 값**이 전달 된다.
따라서, 값이 변경되게 되더라도 실제로 호출된 변수의 값은 바뀌지 않는다.

반면, 
참조 타입의 경우에는 **참조에 의한 호출 방식**, 즉 함수를 호출할 때 인자로 참조 타입인 객체를 전달할 경우, 복사되지 않고 인자로 넘긴 객체의 참조값이 그대로 함수 내부에 전달된다.
따라서, 함수 내부에서 참조값을 이용하여 인자로 넘긴 실제 객체의 값을 변경할 수 있게 되는 것이다.


## 프로토타입
자바스크립트의 **모든 객체는 자신의 부모 역할을 하는 객체와 연결되어 있다.**
객체지향의 상속 개념과 같이 부모 객체의 프로퍼티를 마치 자신의 것처럼 쓸 수 있는 특징이 있다.

이러한 **부모객체를 프로토타입 객체(프로토타입)**이라고 부른다.

![](https://images.velog.io/images/jotang/post/a217488d-6235-45e4-9342-de01119c3566/image.png)

위의 코드에서,
aa 객체를 생성하고, `aa.toStirng()` 메서드를 호출하였다.
toSting이라는 메서드를 정의한 적이 없기에 error가 발생해야 하지만, 정상적으로 결과가 출력된다. 이는 **aa객체의 프로토타입**에 toString()메서드가 이미 정의되어 있기 때문이다.

aa 객체를 살펴보면, name과 age이외에 `__proto__`란 이름의 프로퍼티가 있다는 것을 확인할 수 있다.

이 프로퍼티가 aa객체의 부모인 프로토타입 객체를 가리키고, 그 안에 이미 정의되어있는 메서드들을 사용할 수 있다.

ECMAScript에서는 **모든 객체는 자신의 프로토타입을 가리키는 [[Prototype]] 라는 숨겨진 프로퍼티**를 가진다고 설명하고 있다. 

크롬 브라우저에서는 `__proto__` 가 **[[Prototype]]** 이다.