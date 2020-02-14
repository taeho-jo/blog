---
title: 'AJAX & API & JWT'
date: '2019-12-18'
category: 'web'
---

# AJAX

### Asynchronous : 비동기

- API를 javascript에서 동기로. 호출하는 방법
  (특정 API를 호출)

### 동기와 비동기 알아보기

- 비동기 한줄한줄 실행되고 동기는 실행해야하는 이벤트나 어떠한 명령이 있닫면 그 명령이 끝나야 넘어간다.
- 비동기 API요청 시 request를 보내고 다음줄로 넘어가서 계속해서 실행하다가, request가 확인되고 데이터를 받게 되면 다시 그 코드로 돌아가게된다.

- 게시판을 예로 들어, 예전에는 게시판의 2번페이지를 누르면 URL자체가 바뀌어서 HTML부터 모든 페이지를 다시 rendering하게 된다.
- 요즘에는 게시판의 2번 페이지를 누르면 다른 부분은 제외하고 게시판에 해당하는 페이지만 바뀌게 된다.

### JSON

- JavaScript Object Notation
- Backend의 언어의 종류는 많고 다양한 반면, Frontend는 JavaScript가 많이 이용되기 때문에 JavaScript에서 쓸수있는 데이터 형식으로 만들어 주는 것을 말한다. 여기서 데이터란 TEXT데이더를 말한다.

- JSON => **파일명.JSON**

- JSON이가진 문법

```json
{
  "name": "test",
  "data": [
    {
      "content": "다시 시작..",
      "date": "20190401"
    },
    {
      "content": "첫 트윗!",
      "date": "20181201"
    }
  ]
}
//무조건 ""(쌍따음표, key와 value 모두)
// { } 객체
```

- Backend 와 Frontend 는 JSON형식의 데이터를 예쁘고, 효율적인 형태로 받기위해서 대화를 굉장히 많이 나누어야한다.

# API 호출

- 마켓컬리 카데고리 API

![api실습1.JPG](https://images.velog.io/post-images/jotang/7c1cd410-21b6-11ea-87e7-81339bc25ee5/api실습1.JPG)

- fetch() 함수로 API주소를 호출한다.

![api실습2.JPG](https://images.velog.io/post-images/jotang/7ff81270-21b6-11ea-87e7-81339bc25ee5/api실습2.JPG)

# JWT 로그인 구현

- React에서의 API

```jsx
aaa = (e) => {
    e.preventDefault()
    fetch('http://10.58.1.199:8000/user/auth', {
      method: 'POST',
      body: JSON.stringify({
          email: this.state.id,
          password: this.state.password
      })
    }) //Promise<Response>
    .then(res => res.json())  //Promise<any>
    .then(res => {
      console.log(res.access_token)
    })
  }
// JSON형식으로 변환한다.
// JSON.stringfy()
onClick={this.aaa} // 해당 로그인 버튼 onClick이벤트를 주고 함수를 호출한다.
```

- JavaScript에서의 API

```javascript
loginBtn.addEventListener('click', e => {
  const id = idInput.value;
  const pw = pwInput.value;
  e.preventDefault();
  console.log(id);
  console.log(pw);

  fetch('http://10.58.1.199:8000/user/auth', {
    method: 'POST',
    body: JSON.stringify({
      email: idInput.value,
      password: pwInput.value,
    }),
  }) //Promise<Response>
    .then(res => res.json()) //Promise<any>
    .then(res => {
      alert(res.access_token);
    });
});
// DOM을 이용하여 해당 로그인 버튼에 이벤트
// 호출의 형식은 같다.
```

# JWT

- 지난 시간에 이어서 JWT를 저장 하는 방법에 대해서 알아보려고 한다.

1. **Local Storage**

- 해당 도매인에 영구 저장을 하고 싶을 때 Local Storage에 저장한다.

2. **Session Storage**

- 해당 도매인의, 한 Session에서만 저장하고 싶을 때 이용하고, 창을 닫게되면 저장되었던 모든 Data는 사라지게 된다. (은행과 같은 보안이 중요한 사이트에서 이용)

3. **Cookies**

- 해당 도매인에 유효기간의 날짜를 설정하고 그 때까지만 저장을 하고 싶을 때 이용한다.

- 이러한 의사결정은 회사의 기획에 따라 달라지게 된다.
- 개인 정보는 절대 브라우저에 담을 수 없다. 따라서 JWT로 Back과 Front가 data를 주고 받게 되고 User_table의 user_id만 담아서 보내게 된다.
