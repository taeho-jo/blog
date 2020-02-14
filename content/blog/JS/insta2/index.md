---
title: 'Instargram clone2'
date: '2019-12-03'
category: 'JavaScript'
---

# 1.instargram Login page - clone

## - Problems

1. height 설정에 관한 문제

- **px**, **%**, **vh**

2. font(단위/글씨체)

- **px**, **em**, **rem**

## - solution

- CSS의 길이 단위
- **font-relative length**(상대 길이) \* 크기가 고정되지 않고 기준에 따라서 유동적으로 바뀔 수 있는 길이
  - 반응형 웹이 대두되면서 많은 사이트에서 em, rem을 사용 중(브라우저의 기준에 따라)
  - em, rem, %, vw, vh 등이 대표적인 css 상대 단위
  - **1em** = 16px = 0.17in = 12pt = 1pc = 4.2mm = 0.42cm
  - **em 과 rem의 차이점** : em의 단위 기준은 해당 단위가 사용되고 있는 요소의 font-size이고 rem은 최상위 요소, 즉 html 요소의 font-size 속성 값이 기준이 된다.
  - 가급적 rem을 우선적으로 쓰는 것이 좋다.
  - rem의 r은 'root'(최상위)를 뜻한다.
  - rem은 grid system에 유용하게 사용 가능하다.
  - em의 경우 몇 px로 변환될지에 영향을 주는 변수가 많기 때문이다. em을 많이 사용하게 되면 재사용이 어렵고 유지보수가 힘들어지는 경향이있다.
  ```css
  {
    font-size: 20px;
    width: 10em; /* 200px */
    width: 10rem; /* 160px - html의 font-size */
  `
  ```
- **font-absolute length**(절대 길이)
  - px, in, cm, mm : 절대길이
- **vh & vw (vertical height & vertical width)**
  - 많은 반응형 웹은 % 에 의존
  - CSS width 값은 가장 가까운 부모 요소에 상대적인 영향을 받는다.
  - viewport의 width 값과 height 값에 맞게 사용.
  - vh는 height 값의 100분의 1단위 ex)
