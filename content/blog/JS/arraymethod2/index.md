---
title: 'some() / every()'
date: '2020-06-21'
category: 'JavaScript'
---

# 배열 내장 함수
### some() / every()
some과 every 메서드는 배열의 모든 원소에 대해서 실행되지 않는다.

some 메서드는 함수의 반환값이 ture일 때까지만 원소를 확인하고
every 메서드는 함수의 반환값이 false가 되면 false를 반환하고 함수의 실행을 멈춘다.

그러나 콜백 함수가 모든 원소를 검사하고 어느 시점에서도 true 또는 false 를 반환하지 않는다면 some 메서드는 false 를 반환하고 every 메서드는 ture를 반환한다.

```js
let vowels = ['a', 'e', 'i', 'o', 'u'];

const isVowels = vowels.some(vowel => {
  console.log(a) // 'a', 'e', 'i'
  return vowel === 'a'
})

console.log(isVowels) // true
```
```js
let numbers = [2, 4, 6, 59, 10, 13];

const isNumbers = numbers.every(number => {
  console.log(number) // 2, 4, 6, 59
  return (number % 2 === 0)
})
console.log(isNumbers) // false
```
