---
title: 'TypeScript의 기본타입'
date: '2020-03-14'
category: 'TypeScript'
---

# TypeScript

###### 이제 막 TypeScript에 대한 공부를 시작하였기 때문에 조금 틀린 부분이 있을 수 있습니다. 틀린 부분이 있다면 댓글로 알려주시면 감사드리고, 혹시 더 공부하다가 틀렸던 부분들이나 잘못된 개념이 있다면 수정하고 보완하도록 하겠습니다.

## 왜 TypeScript 인가

### 동적언와와 정적언어

- 동적언어와 정적언어에 대한 구분은 type을 언제 결정하느냐에 따라 다르다

#### 동적언어 :

- 컴파일 시에 자료형을 정하는 것이 아니고 실행 시에 결정
- Run time 까지 type에 대한 결정을 끌고 가기 때문에 선택의 여지가 있다.
- 하지만 실행 도중에 변수에 예상치 못한 타입이 들어와 type error를 뿜는 경우가 발생한다.

#### 정적언어 :

- 변수에 들어갈 값의 형태에 따라 type을 지정해주어야 하고, 컴파일 시에 자료형에 맞지 않다면 컴파일 에러가 발생한다.
- 컴파일 시에 type을 결정하기 때문에 개발 속도가 빠르다.
- type error로 인한 문제점을 개발단계에서 발견할 수 있어 type의 안정성이 올라간다.

#### JavaScript & TypeScript

우리가 평소에 사용하는 JavaScript는 동적인 언어이다. Run time의 속도는 빠르지만 type의 안정성이 보장되지 않는다. 이에 단점을 보완하기 위해 TypeScript가 만들어졌다.

## TypeScript의 기능

- **cross flatform 지원** : JavaScript가 실행되는 모든 flatform에서 사용할 수 있다.
- **객체 지향 언어** : 클래스, 인터페이스, 모듈 등의 강력한 기능을 제공하고, 순수한 객체 지향 코드를 작성할 수 있다.
- **정적 타입** : 정적 타입을 사용하기 때문에 코드를 입력하는 동안에 오류를 체크할 수 있다.(에디터 또는 플러그인의 도움이 필요)
- **DOM 제어** : JavaScript와 같이 DOM을 제어해 요소를 추가하거나 삭제할 수 있다.
- **최신 ECMAScript 기능 지원** : ES6 이상의 최신 JavaScript 문법을 손쉽게 지원한다.

## 컴파일러 설치

`tsc`명령을 사용하기 위해서는 컴파일러를 설치해야한다.

```bash
$ npm install -g typescript
$ tsc --version
$ tsc ./src/index.ts
// typescript 파일을 경로로 지정하면 해당 파일을 컴파일 하게 된다.

// 단일 프로젝트에서만 사용하길 희망하는 경우에는 npx tsc 명령으로 실행할 수도 있다.
```

- 컴파일을 위한 다양한 옵션을 지정할 수 있다.

```bash
$ tsc ./src/index.ts --watch --strict true --target ES6 --lib ES2015,DOM --module CommonJS
```

- tsconfig.json 파일로 옵션을 관리할 수 있다.

```json
{
  "include": [
    "src/**/*.ts" // 컴파일에 포함할 경로.(src폴더안의 모든 파일)
  ],
  "exclude": [
    "node_modules" // 컴파일에 제외할 경로.(노드 모듈은 예외)
  ],
  // 타입스크립트 옵션
  "compilerOptions": {
    "module": "es6", // ts의 형태
    "rootDir": "src",
    "outDir": "dist", // 컴파일된 js파일을 dist 폴더에 저장
    "target": "es6", // js 의 형태
    "sourceMap": true, //ts파일을 바로 콘솔창에서 볼수있음
    "removeComments": true, // 주석의 내용은 컴파일 하면 지워지게 해놓음
    "noImplicitAny": true // 타입 지정 안하면 에러뜨게(any사용을 하지 않기 위해)
  }
}

// 더 많은 옵션들이 있다.
```

# typeScript의 기본타입

TypeScript에 대해 깊게 공부하기 전 기본 타입에 대해서 알아보려고 한다.

## 기본 type과 type의 선언

### 1. Boolean

true or false

```ts
let isBoolean: boolean;
let isDone: boolean = false;
```

### 2. Number

모든 부동 소수점 값을 사용할 수 있다.
ES6에 도입된 2진수 및 8진수 리터럴도 지원

```ts
let num: number;
let integer: number = 6;
let float: number = 3.14;
let hex: number = 0xf00d; // 61453
let binary: number = 0b1010; // 10
let octal: number = 0o744; // 484
let infinity: number = Infinity;
let nan: number = NaN;
```

### 3. String

문자열.
'', "", `` 을 지원한다.

```ts
let str: string;
let red: string = 'Red';
let green: string = 'Green';
let myColor: string = `My color is ${red}.`;
let yourColor: string = 'Your color is' + green;
let plus: string = `This number is ${1 + 3}`; // This number is 4
```

### 4. Array

두 가지의 방법으로 타입을 선언할 수 있다.

```ts
// 문자열만 가지는 배열
let name: string[] = ['태호', '재영', '성진'];
// Or
let name: Array<string> = ['태호', '재영', '성진'];

// 숫자만 가지는 배열
let numberArr: number[] = [1, 2, 3, 4, 5, 6, 7];
// Or
let numberArr: Array<number> = [1, 2, 3, 4, 5, 6, 7];
```

Union 타입(다중 타입)의 문자열과 숫자를 동시에 가지는 배열도 선언이 가능

```ts
let array: (string | number)[] = ['태호', 1, 2, '재영', '성진', 3];
// Or
let array: Array<string | number> = ['태호', 1, 2, '재영', '성진', 3];
```

배열이 가지는 항목의 값을 단정지을 수 없다면 any를 사용할 수 있다.

```ts
let someArr: any[] = [0, 1, {}, [], 'string', false];
```

Interface나 커스텀 타입을 사용할 수 있다.

```ts
interface User {
  name: string;
  age: number;
  isVerified: boolean;
}
let userArr: User[] = [
  {
    name: 'jotang',
    age: 32,
    isVerified: true,
  },
  {
    name: 'jaeyoung',
    age: 26,
    isVerified: false,
  },
];
```

특정값을 타입을 대신해 작성하거나, 읽기 전용 배열을 생성할 수 있다.

```ts
let arr = 10[];
arr = [10];
arr.push(10);
arr.push(11) // error

// 읽기 전용 배열
// readonly 키워드 또는 ReadonlyArray 타입을 사용
let arr1: readonly number[] = [1, 2, 3]
let arr2: ReadonlyArray<number> = [1, 2, 3]
```

읽기 전용 배열은 수정이 불가능하고 읽기만 가능하다.

### 5. Object

`typeof`연산자가 object로 반환하는 모든 타입을 나타낸다. 다만 컴파일러 옵션 설정시에 strict를 true로 설정하면, null은 포함하지 않는다.

```ts
let obj: object = {};
let arr: object = [];
let func: object = function() {};
let nullValue: object = null;
let date: object = new Date();
// ...
```

object는 여러 타입의 상위 타입이기 때문에 유용하지는 않다. 보다 정확한 타입 지정을 위해서 object의 속성들에 대한 타입을 개별적으로 지정한다.

```ts
let user: { name: string; age: number } = {
  name: 'jotang',
  age: 32,
};

// 지정하지 않은 속성이 있다면 error
```

반복적인 사용을 하는 경우, Interface나 type을 사용한다.

```ts
interface User {
  name: string;
  age: number;
}

let user: User = {
  name: 'jotang',
  age: 32,
};
```

### 6. Null & Undefined

null과 undefined는 모든 타입의 하위 타입으로 모든 타입에 할당 할 수 있다. 서로의 타입에도 할당이 가능하다.

- 컴파일 옵션 설정 시 `"strictNullChecks": true`를 설정한다면, 서로의 타입에는 할당할 수 없게 된다.

### 7. Void

값을 return 하지 않는 함수에서 사용한다.
`:void` 의 위치는 함수가 return 하는 값의 타입을 지정하는 부분이다.

```ts
function user(name: string): void {
  console.log(`user name is ${name}`);
}
```

- 값을 return 하지 않는 함수는 undefined를 return 한다.

### 8. Function

화살표 함수를 이용하여 타입을 지정해 줄 수 있다. 인수의 타입과 return 값의 타입을 지정해준다.

```ts
// myFunc는 2개의 숫자 타입 인수를 가지고, 숫자 타입을 반환하는 함수.
let myFunc: (arg1: number, arg2: number) => number;
myFunc = function(x, y) {
  return x + y;
};
```

### 9. Tuple

배열과 유사하지만, 항목의 타입과 항목의 수(배열의 길이)를 지정한다.

```ts
let user: [string, number];
user = ['jotang', 32];
user = ['jotang', 32, 20]; // error
user = [1, 'jotang']; // error
```

데이터들을 개별 변수로 각각 지정하지 않고, 단일 tuple 타입으로 지정하여 사용할 수 있다.

```ts
let userAge: number = 32;
let userName: string = 'jotang';
let isVerified: boolean = true;

let user: [number, string, boolean] = [32, 'jotang', true];
console.log(user[0]); // 32
console.log(user[1]); // 'jotang'
console.log(user[2]); // true
```

tuple 타입의 배열(2차원 배열) 을 사용할 수 있다.

```ts
let users: [number, string, boolean][];
// Or
let users: Array<[number, string, boolean]>;

users = [[1, '태호', true], [2, '재영', false], [3, '성진', true]];
```

- tuple은 배열과 마찬가지로, 특정한 값을 타입 대신 지정해 줄 수 있다.
- tuple은 정해진 항목의 수(배열의 길이)가 있지만, `push(), splice()` 등을 통해 값을 넣는 것을 막지는 못한다. 다만, 기존에 정해진 타입이어야한다.
- readonly 를 이용하여 읽기 전용 tuple을 생성할 수 있다.

### 10. Any

Any는 모든 타입을 의미한다.

- 기존의 JavaScript의 변수와 동일하게 어떠한 타입의 값도 할당할 수 있다.
- 불가피하게 단언할 수 없는 경우에 유용할 수 있다.
- 다양한 값을 포함하는 배열에 사용할 수도 있다.
- Any의 사용을 엄격하게 금지하려면, 컴파일 옵션 설정 시 `"noImplicitAny": true`를 설정하게 되면 any 사용 시 에러를 발생시킬 수 있다.

### 11. Never

never는 **절대 발생하지 않을 값**. 어떠한 타입도 적용할 수 없게 된다.

```ts
function error(message: string): never {
  throw new Error(message);
}

// 빈 배열을 타입으로 잘못 지정한 경우, never를 만날 수 있다.
const never: [] = [];
never.push(3); // error
```

### 12. Union(유니언)

2개 이상의 타입을 허용하는 경우에 Union이라고 한다.
`|` 를 통해 타입을 구분하고, `()` 경우는 선택 사항이다.

```ts
let union: string | number;
union = 'Hello type!';
union = 123;
union = false; // error
```

### 13. Enum(열거형)

숫자 혹은 문자열 값 집합에 이름(Member)을 부여할 수 있는 타입이다.
값의 종류가 일정한 범위로 정해져 있는 경우 유용하다.

##### enum에 대해서 아직 이해가 부족하여, 추후에 업데이트 하도록 하겠습니다.

##### 오늘은 TypeScript가 무엇인지 type의 종류에는 무엇이 있는지 알아보았다.

##### 다음에는 타입추론(Inference), 타입단언(Assertions), 타입가드(Guards)에 대해서 공부해보겠습니다.
