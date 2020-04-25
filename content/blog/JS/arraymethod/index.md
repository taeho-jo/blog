---
title: 'Array methods & DOM & event'
date: '2019-11-30'
category: 'JavaScript'
---

### 1.2 map()

- 배열을 반복, 배열 안의 각 요소를 변환 할 때 사용.
- map() method 의 return 값은 수정된 값으로 다시 생성된 배열.
- array type의 데이터 요소 갯수 만큼 반복
- 반복할 때마다 실행할 함수를 parameter로 전달

```javascript
const array = [1, 2, 3, 4, 5, 6, 7, 8];
const square = n => n * n;
const squared = array.map(square);
// 조금 더 짧게
const squared = arry.map(n => n * n);
console.log(squared);
```

# 2. DOM / event

### 2.1 JavaScript의 위치

- JavaScript 작성

```html
<!-- HTML 문서 내부에 작성 -->
<script>
  function say() {
   		console.log('여기는 index.html파일입니다.')
</script>
<!-- HTML 문서에 링크 -->
<script src="index.js"></script>
<!-- JavaScript 위치 -->
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width" />
    <title>repl.it</title>
  </head>
  <body>
    <h1>html 페이지</h1>
    <script src="index.js"></script>
    <script>
      function say() {
        console.log('여기는 index.html파일입니다.');
      }
      sayHi();
    </script>
  </body>
</html>
```

### 2.2 DOM

- DOM ( **Document Object Model** )
- HTML/CSS 와 JavaScript를 서로 이어주는 역할
- **document** 라는 전역객체를 통해 접근
- document 객체로 element, attribute에도 접근 할 수 있다
- class, id 추가, style 수정 등등 가능

```javascript
const create = document.createElement('p'); //"createElement" , p Tag 생성,
create.className('class'); // className : 클래스 생성
create.innerHTML('wow'); // innerHTML : 요소에 텍스트 작성
// HTML에서 box라는 class를 가진 요소에 접근
const hTag = document.querySelector('.box');
hTag.appendChild(create); //hTag 요소 내부에 create 추가
```

### 2.3 event

- event
- 특정 요소에 interactive한 반응을 할 수 있게 하는 것을 event
- addEventListener
- event를 위해 사용하는 함수 이름

```javascript
요소.addEventListener(이벤트종류, function() {
  //이벤트가 일어났을 때 실행할 내용
});
```

- event 종류
- https://www.w3schools.com/js/js_htmldom_eventlistener.asp
- https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener

`click`, `contextmenu`, `mousedown`, `mouseenter`, `mouseleave` 등등
