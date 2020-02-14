---
title: 'Enemyrain ( Math.random(), getComputedStyle() )'
date: '2019-12-06'
category: 'JavaScript'
---

# 1 . Problems & Solution

### 1. problems

1. ghost의 top value를 알고 싶지만 정보를 알 수 없었다.

2. ghost의 current top value에 px을 더해야하는데 current top value를 찾지 못했다.

### 2. solution

1. JavaScript가 css style속성을 가져올 때 값을 가지고 못하는 경우가 있다. JS가 style 속성을 가져올 때는 inlin Style로 정의된 속성만을 가져오게 된다. 이를 해결해주는 getComputedStyle() 함수를 사용하여 해결하였다. getComputedStyle함수에 대해서는 밑에서 자세히 알아보도록 하겠다.

2. ghost가 밑으로 이동하기 위해서는 current top value에 px를 더해줘야하는데 계속해서 0에 10px을 더하는 상황이였고 `ghostTop++ + 'px'` 를 사용하여 ghost top 값을 계속해서 더하였다.

# 2. Math.random()

- Math.random() 함수를 이용하여 무작위로 숫자를 출력할 수 있다.
- Math.random() 함수는 0~1사이의 숫자를 무작위로 반환하기 때문에 \* 를 이용하여 범위를 지정할 수 있다.

```javascript
let number = Math.random() * 10;
console.log(number); // 1~10 사이의 소수점 자리를 포함하여 랜덤한 숫자를 반환
```

- Math.floor() 를 이용하여 정수를 반환 할 수 있다.

```javascript
let number = Math.floor(Math.random() * 10);
console.log(number); // 1~9 중 random 으로 반환
let number1 = Math.floor(Math.random() * 10 + 1);
console.log(number1); // 1~10 중 random 으로 반환
```

# 3. getComputedStyle()

- inline Style로 정의된 속성이외의 외부 style sheet를 가져오게한다.
- 인자로 전달받은 요소의 모든 CSS속성값을 담은 객체를 전달해주게 된다.
- 너비 또는 높이같은 것은 getComputedStyle 말고 도 offsetWidth(offsetHeight) / scrollWidth(scrollHeight) / clientWidth(clientHeight) 등을 이용할 수 있다.

![캡처.JPG](https://images.velog.io/post-images/jotang/be2cdd00-190a-11ea-9838-d539f766794d/캡처.JPG)

```javascript
let ghostTopValue = getComputedStyle($ghost);
//$ghost의 외부 style sheet의 CSS속성값을 가져온다
```
