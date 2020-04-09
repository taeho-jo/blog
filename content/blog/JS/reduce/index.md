---
title: 'reduce의 활용'
date: '2020-04-09'
category: 'JavaScript'
---

# reduce
배열 내장 함수 중에 하나인 `reduce`는 잘 사용 할 줄 안다면 정말 유용하다.

가장 기본이 되는 총합을 구하는 코드를 살펴보자

```js
const number = [1, 2, 3, 4, 5];

let sum = number.reduce((acc, cur) => acc + cur);
console.log(sum) // 15
```

reduce함수는 두개의 파라미터를 전달한다.

```js
jotang.reduce(() => {...}, [])
```
- `() => {}` 첫번재 인자로는 콜백함수를 받는다.

이 콜백 함수는 **accumulator**와 **current**를 파라미터로 가져온다
```js
(acc, cur) => {...}
```
accumulator는 누적되는 값을 말하고,
current는 배열을 돌면서 해당되는 index의 값을 의미한다.

두번째 파라미터는 reduce함수에서 사용 할 초깃값을 의미한다.
`[]`, `{}`, `number`, `string` 을 초깃값으로 지정할 수 있다.

```js
// string
const jotang = ['jotang', '태호', '조태호'];

let who = jotang.reduce((acc, value) => {
  return acc + value
}, '잘생긴')

console.log(who) // 잘생긴jotang태호조태호

// []
let who = jotang.reduce((acc, value) => {
  acc.push('잘생긴' + value)
  return acc
}, [])

console.log(who) // [ '잘생긴jotang', '잘생긴태호', '잘생긴조태호' ]

//{}
let who = jotang.reduce((acc, value) => acc.concat([{
  잘생긴: value
}]),[])

console.log(who) // [ { '잘생긴': 'jotang' }, { '잘생긴': '태호' }, { '잘생긴': '조태호' } ]
```

reduce함수는 배열을 처음부터 끝까지 반복하면서 콜백 함수가 호출된다.


# Data를 reduce로 조작하기
```js
const insightData = {
  daily : [
    {
      clicks: 10,
      comments: 430,
      followers: 24394,
      likes: 1398,
      theDate: '2020-04-01',
      views: 38
    },
    {
      clicks: 10,
      comments: 430,
      followers: 24394,
      likes: 1398,
      theDate: '2020-04-02',
      views: 38
    },
    {
      clicks: 10,
      comments: 430,
      followers: 24394,
      likes: 1398,
      theDate: '2020-04-03',
      views: 38
    },
    {
      clicks: 10,
      comments: 430,
      followers: 24394,
      likes: 1398,
      theDate: '2020-04-04',
      views: 38
    },
  ]
}
```
```js
let sapceName = insightData.daily.reduce((acc, value) => acc.concat([{
  x: value.theDate,
  visit: value.views,
  click: value.clicks,
  like: value.likes,
  follower: value.followers
}]),[])

console.log(sapceName)
```
```js
[
  {
    x: '2020-04-01',
    visit: 38,
    click: 10,
    like: 1398,
    follower: 24394
  },
  {
    ...
  }
    ...
]
  ```
위와 같은 형태로 바뀌게 된다.
 
이제 다시 위의 형태의 데이터를 또 바꿔보겠다

```js
let chart = spaceName.reduce((acc, value) => {
  const [x, views, clicks, likes, followers] = [...acc];
  
  x.push(value.x)
  views.push(value.visit)
  clicks.push(value.click)
  likes.push(value.like)
  followers.push(value.follower)
  
  return acc
},[
  ['x'],
  ['views'],
  ['clicks'],
  ['likes'],
  ['followers']
])
```
결과값은
```js
[
  [ 'x', '2020-04-01', '2020-04-02', '2020-04-03', '2020-04-04' ],
  [ 'views', 38, 38, 38, 38 ],
  [ 'clicks', 10, 10, 10, 10 ],
  [ 'likes', 1398, 1398, 1398, 1398 ],
  [ 'followers', 24394, 24394, 24394, 24394 ]
]
```

reduce를 활요할 수 있는 방법에 대해서 학습해보았다.

조금 더 익숙해져야겠지만, 많은 방식으로 reduce함수를 이용할 수 있을 것 같다.
