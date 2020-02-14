---
title: 'justSell Project2'
date: '2020-01-14'
category: 'react'
---

# Problems & Solution

## problems

### state관리

![image.png](https://images.velog.io/post-images/jotang/57bf1d80-36d8-11ea-903d-bfdafaa50b6a/image.png)

- 똑같은 styled-components의 3개의 box가 있고, mouse hover가 될 때, font의 color, background-color가 변하는 부분은 styled-components의
  `&:hover`을 이용하였지만, 화살표이미지와 하단의 icon 이미지의 url을 바꾸는 것에서 관리하는 state의 수가 많았다.

### 로그인

![image.png](https://images.velog.io/post-images/jotang/ef27f2f0-36d8-11ea-903d-bfdafaa50b6a/image.png)

- 로그인 시에 access_token을 sesstionStorage에 저장을 하고, 저장이 된 후에 로그인 시에 나와야하는 admin page로 이동하여야 하는데,
  Link를 사용했을 때는 access_token을 저장하지 않고, 바로 page 이동이 일어났다.

## solution

### state 관리

- 하나의 state로 관리를 한다.
- true / false 보다는 stirng을 이용하여 관리한다.

```jsx
const [isActive, setIsActive] = useState('');

const handleHover = e => {
  if (e.target.id === '1') {
    setIsActive('order');
  } else if (e.target.id === '2') {
    setIsActive('report');
  } else if (e.target.id === '3') {
    setIsActive('charge');
  }
};
const handleHoverLeave = () => {
  setIsActive('');
};
```

```jsx
<Arrow
 	 whiteArrow={
    	isActive === "order"
      	? "/assets/images/index/icon_next_w.png"
   	   : "/assets/images/index/icon_next.png"
  }
```

- target의 id값을 이용하여 해당 id일 때, setIsActive를 이용하여 특정 string을 주어 state를 변화 시키고,
  밑에서 삼항연산자를 이용하여 background-image의 url을 바꾼다.

### 로그인

- POST의 response로 access_token을 확인하고, 그 값이 true이라면 sesstionStorage에 저장을 하고, moving()함수를 실행한다.
- Router에서 Link와 history push 가 있는데, nextJS에서는 Router.push를 사용한다고 한다.
- import Router from 'next/router'를 해주고 moving함수에 이동할 경로를 Router.push를 이용하여 넣어준다.

```jsx
import Router from 'next/router';

const moving = () => {
  Router.push('/');
};

const onSubmit = e => {
  console.log('Id: ', userId, 'pw: ', userPw);
  fetch('http://3.133.82.146:8080/user/signin', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({
      user_email: userId,
      password: userPw,
    }),
  })
    .then(response => response.json())
    .then(response => {
      if (response.access_token) {
        sessionStorage.setItem('access_token', response.access_token);
        moving();
      }
    })
    .catch(err => console.log(err));

  setUserId('');
  setUserPw('');
  e.preventDefault();
};
```
