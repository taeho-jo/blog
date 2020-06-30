---
title: '알고리즘, 200630'
date: '2020-06-30'
category: 'agorithm'
---

# 이진연산, 인코딩
```
문제

아스키코드(ASCII)를 이용하여 문자열로 변환하라

'   + -- + - + -   '
'   + --- + - +   '
'   + -- + - + -   '
'   + - + - + - +   '

해(1)와 달(0)
```

```js
let data = [
  '   + -- + - + -   ',
'   + --- + - +   ',
'   + -- + - + -   ',
'   + - + - + - +   ',
  ]

for(let s of data) {
  let answer = '';
  answer += String.fromCharCode(parseInt(
    s.replace(/ /g, '').replace(/\+/g, '1').replace(/-/g, '0'),2))
}
```

* `.replace(/ /g, '').replace(/\+/g, '1').replace(/-/g, '0')`
	정규표현식을 사용하여, 문자사이의 공백을 없애주고, + 는 1로 - 는 0으로 변환한다.
  
  
* `parseInt(String, 2)`
	문자열을 숫자로 변환하여 주면서, 2진법임을 나타내준다.
    
    
* `String.fromCharCode()`
String.fromCharCode() 메서드는 UTF-16 코드 유닛의 시퀀스로부터 문자열을 생성해 반환


# JSON 처리
```
문제
1. 돌다리는 배열로 주어진다.
2. 각 돌다리는 내구도를 가지고있다.
3. 동물들이 다리를 건너게 되고,
4. 동물들이 지나가면 돌다리의 내구도는 동물의 몸무게 만큼 감소한다.
5. 돌다리의 내구도가 동물의 몸무게보다 작으면 동물은 물에 빠진다.
6. 각 동물들은 점프력을 가지고 있다.
7. 살아남은 동물은?
```

```js
const stoneBridge = [5, 3, 4, 1, 3, 8, 3]
const animals = [
  {
    name : "루비독",
    age : "95년생",
    jump : "3",
    weight : "4",
  },
  {
    name : "피치독",
    age : "95년생",
    jump : "3",
    weight : "3",
  },
  {
    name : "씨-독",
    age : "72년생",
    jump : "2",
    weight : "1",
  },
  {
    name : "코볼독",
    age : "59년생",
    jump : "1",
    weight : "1",
  },
]
```

```js
function aliveAnimals(stoneBridge, animals) {
  let answer = [];
  for (let animal of animals) {
    let currentLocation = 0;
    let fail = false;
    while(currentLocation < stoneBridge.length -1) {
      currentLocation += parseInt(animal.jump);
      stoneBridge[currentLocation - 1] -= parseInt(animal.weight)
      if(stoneBridge[currentLocation - 1] < 0) {
        fail = true;
        break
      }
    }
    if(!fail) {
      answer.push(animal.name);
    }
  }
  return answer;
}

aliveAnimals(stoneBridge, animals) // [ '루비독', '씨-독' ]
```