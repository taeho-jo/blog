---
title: 'Enemyrain ( setInterval, setTimeout )'
date: '2019-12-07'
category: 'JavaScript'
---

# 1. setInterval() / clearInterval()

- setInterval() : 일정한 시간 간격으로 작업을 수행하기 위해서 사용하게 된다. setInterval 함수를 중지하긱 위해서 clearInterval 함수를 사용할 수 있다. 주의할 점은 일정한 시간 간격으로 실행되는 작업이 그 시간 간격보다 오래걸릴 경우에 문제가 발생할 수 있다.
- setInterval의 기본 문법

```javascript
setInterval(function() {
  //수행할 작업
}, 1000); //시간(ms으로 작성, 1000ms = 1초)
```

- 이미지를 일정한 간격으로 바꾸어 적용하는 경우 / 일정한 간격으로 배너광고를 보여주는 경우 / 일정 주기로 계속해서 서버와 통신이 필요한 경우 <---이 외에도 굉장히 많이 사용된다.

```javascript
setInterval(function() {
  const newGhost = new Enemy();
}, 1000); //시간(ms으로 작성)
// class에 있는 귀신 생성 함수를 1초에 한번 씩 반복 생성
```

# 2. setTimeout() / clearTimeout()

- setTimeout() : 일정한 시간 후에 작업을 호출한다. setInterval과 달리 지정된 시간을 기다린 후에 작업을 수행하게 되고, 다시 일정한 시간을 기다린 후에 작업을 수행하게 되는 방식이다. clearTimeout을 이용하여 작업을 중지할 수있다.
- clearTimeout() / clearInterval() 은 실행중인 작업을 중지시키는 것이아니라 실행된 이후의 다음 작업스케줄이 중지 되는 것이다.
- setTimeout의 기본 문법

```javascript
setTimeout(function() {
  //수행할 작업
}, 1000);
```

# 3. Problems & Solution

### 1. problems

1.  ghost의 움직임이 멈추고 image까지 바뀌었지만 사라지지 않는다.

2.  hero & ghost 가 만나는 겹치는? 충돌? 하는 부분의 설정.

### 2. solution

1.  ghost가 생성되었을 때 appendChild 했던 것과 반대로 removeChild를 이용한다. ghost가 생성되고 일정 시간이 지난 후에 사라질 수 있도록 setTimeout으로 설정하였다.

```javascript
setTimeout(function() {
  $background2.removeChild($ghost);
}, 10000);
```

2. hero의 left 값과 ghost의 left 값을 가져온다. ghost.left의 value가 hero.left의 value 보다 크거나 ghost.left의 value가 heor.left + width보다 작다면 서로 겹쳐있다는 계산이 나오게 된다. 이렇게 되었을 때 는 ghost가 땅으로 닿기도 전에 죽는 이미지로 변하기 때문에 Y축 값도 설정해준다. 이 때 는 ghost는 bottom 에서 0px에 위치하고 죽어있는 이미지로 바꾼다.

```javascript
if (
  560 > ghostTop &&
  490 < ghostTop &&
  left > parseInt(hero2Style.left) &&
  left < parseInt(hero2Style.left) + 60
) {
  $ghost.style.bottom = '0px';
  $ghost.style.backgroundPosition = '-45px 6px';
}
```
