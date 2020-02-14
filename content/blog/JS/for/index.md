---
title: 'for, datatype, new Date'
date: '2019-11-27'
category: 'JavaScript'
---

# 1. for문(반복문)

### **1.1 반복문(for)**

- 특정 작업을 반복적으로 할 때 사용
- 코드를 원하는 만큼 반복
- 조건 설정

```javascript
for(초기 구문; 조건 구문; 변화 구문;) {
	코드
}
```

```javascript
for (let i = 0; i < 10; i++) {
  console.log(i);
}
```

`i++`로 1씩 증감하는 형태, `i--`로 1씩 빼는 형태도 가능

### **1.2 배열과 for**

```javascript
const name = ['멍멍이', '야옹이', '멍뭉이'];
for (let i = 0; i < name.length; i++) {
  console.log(name[i]);
}
```

- 배열 안의 요소를 하나씩 나열 가능

# 2. datetype(데이터타입)

### **2.1 typeof**

- typeof 연사자를 통해 변수의 **데이터타입을 알 수 있음.**

```javascript
let msg = 'message';
console.log(typeof msg);
console.log(typeof 100);
```

### **2.2 undefined**

### **2.3 null**

### **2.4 boolean**

- boolean 타입에는 2가지 값

1. **true** : string(비어있지 않는 문자열) / number(0을 제외한 모든 숫자) / object(모든 객체)
2. **false** : 빈문자열 ( `'', ""` ) / 숫자( `0 , NaN` ) / object(`null . undefined` )

### **2.5 String**

`' '` , `" "` 안에 Text.

### **2.6 Number**

### **2.7 Object**

- **object**(객체) : key와 value로 이루어진 데이터

# 3. String / Number

### 3.1 대소문자

- JavaScript는 **대소문자를 구분**

`toUpperCase()`: 대문자로 변환, `toLowerCase()`: 소문자로 변환

```javascript
let name = 'JoTang';
let upperName = name.toUpperCase(); //대문자로 변환(JOTANG)
let lowerName = name.toLowerCase(); //소문자로 변환(jotang)
```

### 3.2 문자 길이

`.length`사용

### 3.3 문자열 찾기

`indexOf(찾고싶은 string)`

- 해당 문자의 index를 알려줌.
- 해당 문자가 없다면 **-1을 반환**.

`slice(시작위치, 끝위치)`

- string을 잘라서 반환.

### 3.4 string -> number

`Number()` , `parseInt()` , `parseFloat()`

### 3.5 number -> string

`toString()` , 연산의 특성을 활용 : `let numberString = 1234 + "";`

# 4. new Date(날짜,시간)

### 4.1 Date 생성자

```javascript
let now = new Date(); // 현재 날짜와 시간, 위치
let year = now.getFullYear(); //현재 년도만
let month = now.getMonth() + 1; //getMonth 현재 달보다 1 작은 값이 반환되므로 주의
let date = now.getDate(); //현재 날짜
let day = now.getDay(); //현재 요일
let hour = now.getHours(); //현재 시간(시)
let min = now.getMinutes(); //현재 시간(분)
```

### 4.2 getTime()

- 날짜의 밀리초 표현을 반환
- 비교연산을 통해 언제가 더 과거인지 판단(값이 작으면 과거)

### 4.3 특정 날짜의 Date

- 특정 날짜의 Date를 반환 받을 수 있다.

```javascript
let date1 = new Date('December 17, 2019 03:24:00');
let date2 = new Date('2019-12-17T03:24:00');
let date3 = new Date(2019, 5, 1);
```

## 그 외

### 수학관련 함수

https://www.w3schools.com/js/js_math.asp (Math 매서드)

### @ 반올림/ 올림 / 내림

`Math.round()` : 반올림 / `Math.ceil()` : 올림 / `Math.floor()` : 내림

### @ 랜덤함수

`Math,random()` : 0.0000 ~ 0.9999 사이의 값에서 랜덤수를 제공
`Math.random() * 10` : 곱하기를 통해 원하는 범위 설정 가능

```javascript
let ramdom = Math.random();
console.log(Math.floor(Math.random()*10);
```

위 코드는 내림함수와 랜덤함수를 이용하여 0~10 사이의 랜덤수를 구할 수 있음.
로또번호를 뽑거나, 이벤트 당첨자를 뽑을 때 유용하게 쓰일 수 있다.
