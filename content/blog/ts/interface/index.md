---
title: 'type Inference,Assertions,Guards & Interface '
date: '2020-03-15'
category: 'TypeScript'
---

##### 시작 전, 오늘도 typescript를 공부하지만 틀린 정보가 담길 수 있으니, 혹시 틀린 부분이 있다면 댓글로 알려주시면 감사드립니다. 앞으로 공부해나가면서 틀린부분이나 부족한 부분들은 계속해서 채워나가도록 하겠습니다.

# 타입 추론(Inference)

## 타입 추론이란?

명시적으로 타입 선언이 되어있지 않은 경우에, TypeScript는 스스로 타입을 추론하여 제공하게 된다. 개념은 굉장히 단순하다

> 추론이란? 어떠한 판단을 근거로 삼아 다른 판단을 이끌어 냄

TypeScript가 타입을 추론하는 경우는 다음과 같다.

- 초기값이 설정된 변수
- 기본값이 설정되어 있는 매개 변수
- 반환 값이 있는 함수

```ts
//초기값이 설정된 변수

let num = 10;
// 숫자 10을 할당함으로 인해서 Number 타입으로 추론한다.
// String 타입의 값은 할당할 수 없게 된다.
num = 'jotang'; // error
```

```ts
// 기본값이 설정된 매개변수 & 반환 값이 있는 함수

function addNum(a: number, b: number = 200): number {
  return a + b;
  // 반환 값 ( a + b ) 이 있다.
}
```

**타입 추론이 엄격하지 않은 타입 선언을 의미하는 것은 아니다**.
이를 활용해서 모든 곳에 타입을 명시할 필요는 없으며, 많은 경우에는 더 좋은 코드 가독성을 제공할 수 있다.

# 타입 단언(Assertions)

## 타입 단언이란?

TypeScript가 타입 추론을 통해 판단할 수 있는 타입의 범주를 넘는 경우, 더 이상 추론하지 않도록 지시할 수 있다. 이것을 `타입 단어` 이라고 하고, 코드를 작성하는 프로그래머가 TypeScript 보다 타입에 대해 더 잘 이해하고 있는 상황을 의미.

> 단언이란? 주저하지 않고 딱 잘라 말함

```ts
function someFunc(a: string | number, isNumber: boolean) {
  if (isNumber) {
    a.toFixed(3); // error
  }
}
```

위 코드는 a, isNumber 이라는 매개 변수를 받는 함수를 정의한 것이다. a는 string이나 number를 받는 union타입이고, isNumber는 숫자라면 true, string이라면 false인 boolean타입니다.
이 때, 우리는 toFixed는 a가 숫자여야 사용할 수 있다는 것을 알 수 있다.
하지만, TypeScript는 isNumber이라는 이름만으로 위의 내용을 추론할 수 없다. 그렇기 때문에 `a가 string인 경우에는 toFixed를 사용할 수 없다` 라고 error를 반환하게 된다.

이러한 문제를 두 가지 방식으로 단언할 수 있다.

```ts
function someFunc(a: string | number, isNumber: boolean) {
  if (isNumber) {
    (a as number).toFixed(3);
    // or
    // (<number>a).toFixed(3)
  }
}
```

다만, 두 번재 방식인 `<number>a`은 JSX를 사용하는 경우에 특정 구문 파싱에서 문제가 발생할 수 있다. 따라서 `.tsx` 파일에서는 사용할 수 없다.

"나는 알고 있으니깐 나를 믿어!" 라고 TypeScript에게 알려주는 것과 같다.

## Non-null 단언 연산자

Non-null 단언 연산자는 `!` 를 사용한다. `!`를 통해 null 이나 undefined 값이 아님을 단언할 수 있고, 변수나 속성에서 간단하게 사용할 수 있기 때문에 유용하다.

타입단언, if조건문으로 해결할 수 있지만, Non-null 단언 연산자(`!`) 를 이용할 수 도 있다.

```ts
function someFunc(a: number | null | undefined) {
  return a.toFixed(2);
}
// error

// 1. 타입 단언
function someFunc(a: number | null | undefined) {
  return (a as number).toFixed(2);
}
function someFunc(a: number | null | undefined) {
  return (<number>a).toFixed(2);
}

// 2. if조건문
function someFunc(a: number | null | undefined) {
  if (a) {
    return a.toFixed(2);
  }
}

// 3. Non-null 단언 연산자
function someFunc(a: number | null | undefined) {
  return a!.toFixed(2);
}
```

# 타입 가드(Guards)

## 타입 가드란?

타입 단언을 여러 번 사용하게 되는 경우, 코드가 지저분해 질 수 있다. 따라서 이 때 타입 가드를 제공하게 되면, TypeScript가 특정 Scope에서 타입을 보장할 수 있게 된다.

#### 타입 가드의 형태

`Name is Type`의 형태, `타입 술부(Predicate)를 반환 타입으로 명시한 함수.`

> 술부란? 주어의 상태, 성질 따위를 서술하는 말

예제를 통해 더 알아보도록 하겠다.

```ts
// 타입가드 (Name is Type === a is number)
function isNumber(a: string | number): a is number {
  return typeof a === 'number';
}

function someFunc(a: string | number) {
  if (isNumber(a)) {
    a.toFixed(2);
    isNan(a);
  } else {
    a.split('');
    a.toUpperCase();
    a.length;
  }
}
```

`typeof`와 `instanceof` 연산자를 직접 사용하는 타입 가드도 있다.
비교적 단순한 로직에서 추천되는 방식.

> typeof 연산자는 number, string, boolean, 그리고 symbol만 타입 가드로 인식할 수 있다.

```ts
// 기존 예제와 같이 `isNumber`를 제공하지 않아도 `typeof` 연산자를 직접 사용하면 타입 가드로 동작합니다.
function someFunc(a: string | number) {
  if (typeof a === 'number') {
    a.toFixed(2);
    isNaN(a);
  } else {
    a.split('');
    a.toUpperCase();
    a.length;
  }
}

// 역시 별도의 추상화 없이 `instanceof` 연산자를 사용해 타입 가드를 제공합니다.
class Cat {
  meow() {}
}
class Dog {
  woof() {}
}
function sounds(ani: Cat | Dog) {
  if (ani instanceof Cat) {
    ani.meow();
  } else {
    ani.woof();
  }
}
```

# 인터페이스(interface)

## 인터페이스란?

TypeScript 객체를 정의하는 일종의 규칙이며 구조.
속성과 메소드 등의 타입을 정의할 수 있다.

```ts
interface User {
  name: string;
  age: number;
  isAdult: boolean;
}

let user1: User = {
  name: 'jotang',
  age: 32,
  isAdult: true,
};
let user2: User = {
  name: 'jaeyoung',
  age: 26,
}; // error (isAdult가 빠졌기 때문)

// interface를 선언할 때, : / , 는 사용하지 않아도 가능하다.
interface User {
  name: string;
  age: number;
}
interface User {
  name: string;
  age: number;
}
interface User {
  name: string;
  age: number;
}
```

제일 처음 예제에서 처럼 타입을 선언했는데, 빠지게 되면 error가 발생한다. 이러한 부분을 보완할 수 있는 것이 `?`이다. 선택적 속성(Opitonal properties)으로 정의할 수 있게 된다.
`필수가 아닌 속성으로 정의` 하는 방법!

```ts
interface User {
  name: string;
  age: number;
  isAdult?: boolean; // Optional property
}

// `isAdult`를 초기화하지 않아도 에러가 발생하지 않습니다.
let user: IUser = {
  name: 'jotang',
  age: 32,
};
```

## 읽기 전용 속성(Readonly properties)

`readonly` 키워드를 사용하면 초기화된 값을 유지해야 하는 **읽기 전용 속성을 정의**할 수 있다.

```ts
interface User {
  readonly name: string;
  age: number;
}

let user: User = {
  name: 'jotang',
  age: 32,
};

user.name = '조탕'; // error
user.age = 20; // ok.
```

정의하는 모든 속성이 readonly일 경우, 유틸리티(Utility) 또는 단언(Asseertion) 타입을 활요할 수 있다.

```ts
// 1. 정의하는 모든 속성에 readonly 키워드 사용
interface User {
  readonly name: string;
  readonly age: number;
}
let user: User = {
  name: 'jotang',
  age: 32,
};
user.name = '조탕'; // error
user.age = 20; // error

// 2. Utility 사용
interface User {
  name: string;
  age: number;
}
let user: Readonly<User> = {
  name: 'jotang',
  age: 32,
};
user.name = '조탕'; // error
user.age = 20; // error

// 3. Type assertion
let user = {
  name: 'jotang',
  age: 32,
} as const;
user.name = '조탕'; // error
user.age = 20; // error
```

20살로 돌아가고싶지만...읽기전용이기에 바꿀 수 없다.

## 인덱싱 가능 타입(Indexable types) & 인덱스 시그니처(Index sinature)

interface를 통해 특정 속성의 타입을 정의할 수는 있지만, 많은 속성을 가지거나 단언할 수 없는 임의의 속성이 포함되는 구조에서는 한계가 있다. 이러한 상황에서 유용한 것이 인덱스 시그니처(Index signature)이다.

- arr[0], obj['name'] 처럼 숫자 또는 문자로 인덱싱하는 인덱싱 가능 타입(Indexable types)이 있다.
- 인덱싱 가능 타입들을 정의하는 interface가 인덱스 시그니처라는 것을 가질 수 있다.

인덱스 시그니처(Index signature) 아래의 구조와 같고, 인덱싱에 사용할 인덱서의 이름과 타입 그리고 인덱싱 결과의 반환 값을 지정해야한다.

```ts
interface Indexer {
  [Indexer_name: Indexer_type]: Return_type;
}

// 인덱서의 타입은 string과 number만 지정할 수 있다!!!!
```

> 배열(객체)에서 위치를 가리키는 숫자(문자)를 인덱스(index)라고 하며, 각 배열 요소(객체 속성)에 접근하기 위하여 인덱스를 사용하는 것을 인덱싱(indexing)이라고 한다.(배열을 구성하는 각각의 값은 배열 요소(element)라고 합니다)

- number로 indexing

```ts
interface Item {
  [itemIndex: number]: string;
}
let item: Item = ['a', 'b', 'c'];
consoel.log(item[0]); // a is string
consoel.log(item[1]); // b is string
consoel.log(item['0']); // error
```

interface Item은 인덱스 시그니처를 가지고있다.
index는 number를 타입으로 지정하였고, 반환은 string으로 해야한다.

union 타입을 사용한다면 활용폭이 커진다.

```ts
interface Item {
  [itemIndex: number]: string | boolean | number[];
}
let item: Item = ['Hello', false, [1, 2, 3]];
console.log(item[0]); // Hello
console.log(item[1]); // false
console.log(item[2]); // [1, 2, 3]
```

- string으로 indexing

```ts
interface User {
  [userProp: string]: string | boolean | any;
}
let user: User = {
  name: 'jotang',
  email: 'jotang3726@gmail.com',
  isValidate: true,
  0: false,
};
console.log(user['name']); // 'jotang' is string.
console.log(user['email']); // 'jotang3726@gmail.com' is string.
console.log(user['isValidate']); // true is boolean.
console.log(user[0]); // false is boolean
console.log(user[1]); // undefined
console.log(user['0']); // false is boolean
```

user[0]과 같은 숫자로 인덱싱하는 경우나 user['0']과 같이 문자로 인덱싱하는 경우 모두 인덱싱 전에 숫자가 문자열로 변환된다.

- 인덱스 시그니처를 사용하면 interface에 정의되지 않은 속성들을 사용할 때 좋다.
- 해당 속성이 인덱스 시그니처에 정의된 반환 값을 가져야 하는 것을 주의해야한다.

```ts
interface User {
  [userProp: string]: string | number;
  name: string;
  age: number;
}
let user: User = {
  name: 'jotang',
  age: 32,
  email: 'jotang3726@gmail.com',
  isAdult: true, // error
};
console.log(user['name']); // 'jotang'
console.log(user['age']); // 32
console.log(user['email']); // jotang3726@gmail.com
```

isAdult 속성은 정의된 string이나 number 타입을 반환하지 않기 때문에 error가 발생한다.

##### 오늘은 타입추론, 타입단언, 타입가드 그리고 interface에 대해서 공부하였다.

##### interface에서 index signature부분에 대한 이해는 조금 더 필요한 것 같다. 앞으로 typescript를 사용해 나가면서 부족한 부분들을 더 채워나가도록 하겠다.
