---
title: 'object & scope'
date: '2019-11-29'
category: 'JavaScript'
---

# 1. object

### 1.1 object(객체)

- 변수 or 상수 하나의 이름에 여러 종류의 값을 넣을 수 있다.
- string, number, 함수, 또 다른 객체 등 **어떤 종류의 값**도 객체안에 넣을 수 있다.

```javascript
const sample = {
  key: value,
  key2: value2,
};
```

- 객체는 **key**와 **value**로 구성
- property 이름은 중복될 수 없다.
- property를 추가할 때는 ,(쉼표)
- key는 기본적으로 공백이 없어야한다. 만약 공백이 있어야한다면 `" "` 로 감싸서 strig으로 넣어주면 된다.

```javascript
const sample = {
  'key with space': ture,
};
```

### 1.2 property 접근

- dot(`.`) 과 `[]`로 property에 접근 할 수 있다.
- `[' ']` : 대괄호로 접근하려면 key이름을 `''`,`""` 로 감싸서 작성

```javascript
let objData = {
  name: 50,
  address: {
    email: 'gaebal@gmail.com',
    home: '위워크 선릉2호점',
  },
  books: {
    year: [2019, 2018, 2006],
    info: [
      {
        name: 'JS Guide',
        price: 9000,
      },
      {
        name: 'HTML Guide',
        price: 19000,
        author: 'Kim, gae bal',
      },
    ],
  },
};
```

```javascript
console.log(objData.books.year[0]); //2019
console.log(obData['books'].year[1]); //2018
```

- object의 key에는 **스페이스, 한글, 특수문자** 등이 들어갈 수 있고, `''`로 감싸준다.

```javascript
let difficult = {
  33: '숫자 형식도 되네',
  'my name': '스페이스 포함 가능',
  color: 'silver',
  키: '한글인 키는 따옴표가 없어도 되는군!!',
  '!키': '느낌표 있는 키는 따옴표가 필요하군',
  $special: '$는 없어도 되는군',
};
// 호출 시에는 대괄호를 사용
```

- 변수로 접근

```javascript
let name = '키';
console.log(difficult[name]); //'한글인 키는 따음표가 없어도 되는군!!'
console.log(difficult.name); //undefined
//dot(.)은 실제 key 이름을 사용할 때
//변수로 접근 할 때는 항상 [] 로 접근
```

- property 값의 교체(수정),추가

```javascript
difficult[name] = '값 바꾼다';
difficult.color = '색깔';
console.log(difficult[name]); //'값 바꾼다'
console.log(difficult.color); //'색깔'
console.log('생성전: ' + difficult.new); //underined
difficult.new = '새로 추가된 프로퍼티';
console.log('생성후: ' + difficult.new); //'생성후: 새로 추가된 프로퍼티'
```

### 1.3 Method

- object에 저장된 값이 함수일 때, method라고 한다.

```javascript
let methodObj = {
  do: function() {
    console.log('메서드 정의는 이렇게');
  },
};
//호출방법
methodObj.do();
```

# 2. scope

### 2.1 scope

- **'변수가 어디까지 쓰일 수 있는지'**의 범위

1.  **global (전역) scope** : 코드의 모든 범위에서 사용 가능
2.  **function (함수) scope** : 함수 안에서만 사용 가능
3.  **block (블록) scope** : if, for, switch 등 특정 블록 내부에서만 사용 가능

### 2.2 Block

- block은 { }로 감싸진 것을 block 이라고 한다.

```javascript
//function
function hi() {
  return 'i am block';
}
//for
for (let i = 0; i < 10; i++) {
  count++;
}
//if
if (i === 1) {
  let j = 'one';
  console.log(j);
}
//block 내부에서 변수가 정의 되면 오로지 block 내부에서만 사용 가능
//local(지역) 변수라고 한다.
```

### 2.3 scope pollution(scope 오염)

- 전역변수가 global namespace 또는 코드블럭에서 값이 수정되어 사용. 해당 변수를 이용하기 어려워지는 것

```javascript
//대표적인 예
function logSkyColor() {
  const dusk = true;
  let myColor = 'blue';
  if (dusk) {
    let myColor = 'pink';
    console.log(myColor); // pink
  }
  console.log(myColor); // blue
}
console.log(myColor); // 에러!!
```

- scoping의 좋은 습관
- 변수들은 block scope으로 최대한 나누기
- 전역변수보다 지역변수를 사용하자.
