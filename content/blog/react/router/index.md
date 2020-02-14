---
title: 'Router'
date: '2020-01-20'
category: 'react'
---

# 경로에 특정 값 넣기

## Router의 3가지 props

1. **history** 이 객체를 통해 push, replace 를 통해 다른 경로로 이동하거나 앞 뒤 페이지로 전환 할 수 있습니다.
2. **location** 이 객체는 현재 경로에 대한 정보를 지니고 있고 URL 쿼리 (/about?foo=bar 형식) 정보도 가지고있습니다.
3. **match** 이 객체에는 어떤 라우트에 매칭이 되었는지에 대한 정보가 있고 params (/about/:name 형식) 정보를 가지고있습니다.

## 두가지 방법 ( params & query )

# params

- params 의 경우엔 사용하기 전에 꼭 라우트에서 지정을 해야한다.

![image.png](https://images.velog.io/post-images/jotang/f6794b30-3c3c-11ea-a902-c51636c21c22/image.png)

- props로 history를 받아오기 때문에, 비구조할당을 통해서 history를 받아온다.

![image.png](https://images.velog.io/post-images/jotang/a52505c0-3c3d-11ea-aaa0-a3acb80bcd7f/image.png)

- movePage 함수에서 인자로 id값을 받아오고, history.push(`/content/${id}`)로 path를 지정한다.
- 받은 history의 push 메서드에 이동할 path를 인자로 넘겨주면, 해당 라우트로 이동할 수 있다.

![image.png](https://images.velog.io/post-images/jotang/40357be0-3c3d-11ea-b762-b7e0b590fa34/image.png)

### withRouter HOC로 구현하는 방법

![image.png](https://images.velog.io/post-images/jotang/d4cb3420-3c3d-11ea-aaa0-a3acb80bcd7f/image.png)

![image.png](https://images.velog.io/post-images/jotang/e4b573f0-3c3d-11ea-aaa0-a3acb80bcd7f/image.png)

- props에 route 정보(history)를 받으려면 export하는 components를 withRouter로 감싸주어야 합니다.

![image.png](https://images.velog.io/post-images/jotang/b944b510-3c41-11ea-a902-c51636c21c22/image.png)

- props로 match를 받아온다.
- match.params.id 을 통하여 확인 할 수 있다.
