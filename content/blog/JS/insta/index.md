---
title: 'Instargram clone (login & main)'
date: '2019-12-02'
category: 'JavaScript'
---

# 1. login page - event

### 1.1 login page event GOAL

- ID, PW에 각 한 글자 이상 시 버튼 활성화.(연한파랑 -> 파랑)

```javascript
const loginBtn = document.querySelector('.loginBtn');
const idInput = document.querySelector('.idInput');
const pwInput = document.querySelector('.password');
const input = document.querySelector('.formBox');
input.addEventListener('keyup', function() {
  const id = idInput.value;
  const pw = pwInput.value;
  if (id.length !== 0 && pw.length !== 0) {
    loginBtn.style.backgroundColor = '#0984e3';
  } else if (id.length === 0 || pw.length === 0) {
    loginBtn.style.backgroundColor = '#C9E0FB';
  }
});
```

### 1.2 problems

- event의 종류
- addEventListener 문법
- .value의 의미

### 1.3 solution

- 공부하고 오겠음
- 공부하고 오겠음
- **idInput.value** / **pwInput.value** : idInput과 pwInput의 값. 그 외에도 `.value`는 **고유의 값**을 의미

# 2. main page - event

### 2.1 main page event GOAL

- 댓글 input 창에 enter or "게시" click 시에 댓글 추가

```javascript
const btn = document.querySelector('.Btn'),
  comInput = document.querySelector('.peopleSay'),
  comBox = document.querySelector('.comment');
function startsHandler(event) {
  if (event.keycode === 13 || event.type === 'click') {
    const pTag = document.createElement('p');
    const spanTag = document.createElement('span');
    pTag.className = 'js-p';
    pTag.innerHTML = 'wecode_bootcamp';
    comBox.appendChild(pTag);
    spanTag.className = 'js-span';
    spanTag.innerHTML = comInput.value;
    pTag.appendChild(spanTag);
    comInput.value = '';
  }
}
function init() {
  btn.addEventListener('keyup', startsHandler);
  btn.addEventListener('click', startsHandler);
}
init();
```

### 2.2 problems

- event 객체
- 순서
- comInput.value = '';
- key code
- event target
- callback function
- 전체적인 공부가 다시 필요

### 2.3 solution

- 이번 주 안에 돌아옵니다. 다른 것들도 순차적으로 해결 다 하겠슴

---
