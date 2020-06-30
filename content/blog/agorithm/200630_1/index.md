---
title: '알고리즘 Lv1, 200630'
date: '2020-06-30'
category: 'agorithm'
---

프로그래머스 알고리즘 Level1 부터 차근히 진행해보려고 합니다.

# 가운데 글자 가져오기
```
문제
단어 s의 가운데 글자를 반환하는 함수를 만드시오.
단어의 길이가 짝수라면 가운데 두글자를 반환하면 됩니다.
* 제한사항 *
s는 길이가 1이상, 100이하인 String

s = 'abcde' || s = 'qwer'
```

```js
function solution(s) {
  let answer = ''
  const length = s.length / 2
  if(s.length % 2 !== 0) {
    answer += s.slice(Math.floor(length), Math.ceil(length))
  }
  else {
    answer += s.slice(length - 1, length + 1)
  }
  return answer
}
```

# K번째 수
```
문제
배열 array의 i번째 숫자부터 j번째 숫자까지 자르고 정렬했을 때, k번째에 있는 수를 구하시오.

예시)
array = [1, 5, 2, 6, 3, 7, 4]
commands =[2, 5, 3]
i=2, j=5, k=3
i에서 j까지 자르고 정렬을 하면 [2, 3, 5, 6]
k번째에 있는 숫자는 5

commands는 2차원 배열이다.
```

```js
function solution(array, commands) {
  let answer = []
  for(let i of commands) {
    answer.push(array.slice(i[0]-1, i[1]).sort((a, b) => {
      return a - b})[i[2] - 1])
  }
  return answer
}
```

* `let i of commands` 후에 i를 console에 확인하면, 각 배열이 출력되는 것을 확인 할 수 있다. 따라서 i[0]은 commands안의 각 배열 요소의 첫번째 요소이다.


# 같은 숫자는 싫어
```
문제
배열 arr은 숫자 0~9로 이루어져있다.
연속적으로 나타나는 같은 숫자는 하나만 남기고 전부 제거.
제거된 후 남은 수들을 반환할 때는 배열 arr의 순서를 유지해야한다.

예시)
arr = [1, 1, 3, 3, 0, 1, 1] 이라면 return은 [1, 3, 0, 1]
```

```js
function solution(arr) {
  let answer = []
  answer[0] = arr[0]
  for(let i = 0; i < arr.length - 1; i++) {
    if(arr[i] !== arr[i + 1]) {
      answer.push(arr[i + 1])
    }
  }
  return answer
}
```
```js
// 다른사람의 풀이
// filter를 사용
function solution(arr) {
  return arr.filter((el, idx) => el !== arr[idx + 1])
}
```

# 두 정수 사이의 합
```
문제
두 정수 a, b가 주어졌을 때, a와 b 사이에 속한 모든 정수의 합을 구하시오

예시)
a = 3, b = 5 ==> 3 + 4 + 5 = 12

* 제한 조건 *
a와 b가 같은 숫자라면 a 또는 b를 반환
a와 b의 대소관계는 정해져있지 않다.
```

```js
function solution(a, b) {
  let answer = 0
  
  if(a < b) {
    for(let i = a; i <= b; i++) {
      answer += i
    }
  }
  else if(a > b) {
    for(let i = b; i <= a; i++) {
      answer += i
    }
  }
  else if(a === b) {
    answer += a
  }
  return answer
}
```

```js
// 다른 사람의 풀이
function solution(a, b) {
  let answer = 0
  for (let i = Math.min(a, b); i <= Math.max(a, b); i++) {
    answer += i
  }
  return answer
}
```

# 서울에서 김서방 찾기
```
문제
String형 배열의 seoul의 요소 중에 'Kim'의 위치 x를 찾아,
'김서방은 x에 있다'는 String을 반환.

* 제한 사항 *
'Kim'은 반드시 seoul안에 포함되어 있고, 오직 한 번만 나타난다.
```

```js
function solution(seoul) {
  let answer = '';
  const location = seoul.findIndex(el => el === 'Kim')
  
  return answer += `김서방은 ${location}에 있다`
}
```

# 수박수박수박수박수박?
```
문제
길이가 n이고, '수박수박수박...'의 패턴을 유지하는 문자열을 반환.

예시)
n이 4라면, '수박수박' / n이 3이라면, '수박수'
```

```js
function solution(n) {
  let answer = ''
  
  for(let i = 1; i <= n; i++) {
    answer += i % 2 === 0 ? '박' : '수'
  }
  return answer
}
```

# 약수의 합
```
문제
정수 n을 입력받아 n의 약수를 모두 더한 값을 반환.
n은 0이상 3000이하인 정수
```

```js
function solution(n) {
  let answer = 0
  
  for(let i = 1; i <= n; i++) {
    if(n % i === 0) {
      answer += i
    }
  }
  return answer
}
```

* `약수`: 어떤 수를 나누었을 때, 나머지가 0이 되는 수, 즉 나누어 떨어지는 수를 의미하고, 약수는 정수여야한다. 모든 자연수는 최소한 자기자신과 1을 약수로 갖는다.

# 평균 구하기
```
문제
정수를 담고 있는 배열 arr의 평균값을 반환
```

```js
function solution(arr) {
  let answer = 0
  
  const addNum = arr.reduce((acc, value) => {
    return acc + value
  }, 0)
  
  answer += c / arr.length
  
  return answer
}
```

# 휴대폰 번호 가리기
```
문제
전화번호의 일부를 가려서 반환
번호가 문자열로 주어졌을 때, 전화번호의 뒷 4자리를 제외한 나머지 숫자를 전부 *로 가린 문자열을 반환.
```

```js
function solution(s) {
  let answer = ''
  
  const number = s.split('')
  
  for(let i = 0; i < number.length - 4; i++) {
    answer += number[i]
    answer = answer.replace(number[i], '*')
  }
  for(let j = s.length - 4; j < s.length; j++) {
    answer += s[j]
  }
  
  return answer
}
```

```js
// 다른 사람의 풀이
// 정규표현식 사용
function hide_numbers(s) {
  return s.replace(/\d(?=\d{4})/g, "*");
}
```

