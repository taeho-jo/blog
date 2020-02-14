---
title: 'Fetch & useEffect'
date: '2020-01-17'
category: 'react'
---

# 로그인 구현하기

![image.png](https://images.velog.io/post-images/jotang/0cbea150-386a-11ea-9f86-495f468882a8/image.png)

- form 태그를 이용하여 onClick 대신 onSubmit event를 사용하였다.
- method는 POST, body에는 user_email과 password를 담아보낸다.

```jsx
.then(res => res.json())
.then(res => {
	if (res.access_token) {
    	sessionStorage.setItem("access_token", res.access_token);
        movint()
    }
})
.catch(err => console.log(err));

setUserId("")
setUserPw("")
e.preventDefault()
```

- 로그인을 요청하고 그 응답으로 access_token이 true라면 'sessionStorage'에 저장을한다.
- 그 이후에 Router.push('/admin') 을 moving 함수로 만들어서 실행한다.

# 데이터 받아오기

```jsx
useEffect(() => {
  fetch('http://localhost:3000/data/reporttable.json')
    .then(res => res.json())
    .then(res => setData(res));
}, []);
```

- useEffect는 class형 component의 componentDidMount와 componentDidUpdate를 합친 형태로 보아도 무방하다.
- useEffect에서 설정한 함수를 컴포넌트가 화면에 맨 처음 랜더링될 때만 실행하고, 업데이트될 때는 실행하지 않으려면 함수의 두 번째 파라미터로는 비어 있는 빈 배열을 넣어주면 된다.

### 특정 값이 업데이트 될 때만 실행하고 싶을 때

```jsx
componentDidUpdate(preProps, preState) {
	if(preProps.value !== this.props.value) {
    	doSomething()
    }
}
//class형

useEffect(() => {
	console.log('name')
}, [name] )
// hooks
```

- class형에서는 props안에 들어 있는 value 값이 바뀔 때만 특정 작업을 수행한다.

- useEffect의 두 번째 파라미터로 전달되는 배열 안에 검사하고 싶은 값을 넣어주면 된다. 두 번째 파라미터로 전달된 값이 변할 때만 실행된다.
- 배열 안에는 useState를 통해 관리하고 있는 상태를 넣어주어도 되고, props로 전달받은 값을 넣어 주어도 된다.
