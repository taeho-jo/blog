---
title: 'Authentication & Authorization'
date: '2019-12-17'
category: 'web'
---

# 1. Authentication(인증)

### 1.1 Authentication이란?

- 인증이란 유저의 identification을 확인하는 절차이다.
- 회원가입, 로그인 등 유저의 아이디와 비밀번호를 확인하는 절차이다.
- open API에도 인증절차가 필요한데, 누가 언제 어떻게 쓰는지 알아야 하기 때문이다.

### 1.2 어떠한 방식으로 인증할 것인가?

- HTTP의 속성 중 stateless. stateless의 특징으로 인해 어떠한 방식으로 id와 password등의 정보를 담아서 보낼 것인가.
- JWT가 어떻게 해서 나오게 되었는가

1.  id와 password의 정보를 담아서 보내야한다. 여기서 문제점은 id와 password가 노출 될 수 있고, 매번 server에서 요청이 올때마다 매번 확인해야한다.
2.  해결을 위해 임시id를 만들어서 보낸다. 여기서도 문제점이 발생하는데 유저가 누구인지 알 수 없고, 실제 유저의 정보를 따로 맵핑해야한다.
3.  다시 생긴 문제점을 해결하기 위해 일정 시간동안 유효한 정보를 담아 보낸다. 실제 유저의 정보를 대체할 수 있는 정보를 보낸다. 대표적인 예로 이메일이 있지만 이 역시 도용이 가능하다는 문제점이 있다.

- 민감한 개인정보가 아니여야하고, 일정시간 동안 유효해야하며, 도용이 될 수 없게 정보를 담아 보내야한다.
- 이러한 문제점들로 인해 JWT(JSON Web Tokens)이 나오게 된다.

### 1.3 JWT(JSON Web Tokens)

- 유저관련 민감한 데이터를 전송할 수 있게 Token화
- table id 를 제공한다.
- table id는 JSON의 형태
- JSON형태의 Front에서 Back으로 정보를 전달하게 된다.

```
{
    "id": 1,
    "epx": "~~~"
}
```

- Back에서 Web상에서 전달할 수 있게 JSON을 Token화 시켜 Front로 보낸다.
- Front에서 JWT를 받아서 쿠키에 저장해 둔다. 재요청시 저장되있던 정보를 포함하여 Back으로 보내게 된다.
- 요청에 대한 부가정보이기 때문에 header에 포함하게 된다.
- Back에서 복호화 한 후에 다시 읽어 해당 유저가 누구인지 알게된다.
- access_token

```
"access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZGVudGl0eSI6MSwiaWF0IjoxNDQ0OTE3NjQwLCJuYmYiOjE0NDQ5MTc2NDAsImV4cCI6MTQ0NDkxNzk0MH0.KPmI6WSjRjlpzecPvs3q_T3cJQvAgJvaQAPtk1abC_E"
```

- `.` 을 기준으로 3부분으로 나뉘게 된다.(sdfas.sdfas.asdfas)
- 1, 2번째 부분은 코드화(m코드화)시키게 되고 마지막 3번째 부분은 암호화 하게된다.
- 코드화와 암호화의 차이 -> 코드화는 누구나 다시 decode할 수있고, 암호화는 해당 JWT를 만든 sever에서만 복호화가 가능하다.

### 1.4 로그인 절차와 비밀번호 암호화

- 로그인 절차

1.  유저의 id와 password 생성
2.  password를 암호화해서 DataBase에 저장
3.  로그인 시 유저가 id와 password 입력
4.  유저가 입력한 id와 password를 암호화 한 후 암호화 되서 DB에 저장된 유저의 id, password와 비교
5.  일치한다면 로그인 성공
6.  로그인 성공시 access token을 클라이언트에게 전송
7.  유저는 로그인 성공 후 access token을 첨부해서 request를 서버에 전송함으로서 매번 로그인 하지 않아도 된다.

- password 암호화
- password는 암호화해서 DB에 저장하게 된다. 그 이유는 DB가 해킹을 당했을 때 그대로 노출되게 되고, 내부유출에 대비하기 위해서이다.
- 비밀번호는 단방향 해쉬 함수로 암호한다.(one-way-hash function)

```
test password
-> 0b47c69b1033498d5f33f5f7d97bb6a3126134751629f4d0185c115db44c094e
```

- 기존의 값을 알 수 없다.
- server에서 유저가 password를 제대로 입력했는지 확인하기 위해서 유가자 password를 입력하면 똑같이 암호화해서 보내게 되고, 일치하는 지를 확인하게 된다.

### 1.5 Bcrypt

- Rainbow table attack - 미리 해쉬값들을 계산해 놓은 table을 Rainbow table이라고 한다.
- 미리 게산된 table을 가지고 있으면 역추적만 한다면 쉽게 알 수 있게 된다. 결국 one-way-hash도 노출가능성이 있다.
- hash값을 구하는게 빠르고 hash는 password를 위한 알고리즘이 아니다. 이를 해결하기 위해서 1.salting 2.key chaining이 필요하다.

1. salting : 암호화시에 원본값에 random한 값을 추가시킨다.
2. hash값이 나오면 hash->hash->hash를 N번 반복하게 된다.

![캡처.JPG](https://images.velog.io/post-images/jotang/7ea3b1b0-2024-11ea-a515-016ceffabf7e/캡처.JPG)

# 2. Authorization(인가)

### 2.1 Authorization이란?

- 유저가 요청하는 request를 실행하라 수 있는 권한이 있는 유저인지를 확인하는 절차이다.

### 2.2 Authorizaition의 절차

1. access token을 생성한다.(유저의 정보를 확인할 수 있는 정보가 들어가야한다.)
2. 유저가 request를 보낼 때 access token을 첨부하여 보낸다.
3. server에서 유저가 보낸 access token을 복호화한다.
4. 복화를 통해서 유저 id를 얻고, 이 id를 통해 DB에서 해다아 유저의 권한을 확인한다.
5. 권한을 가지고 있으면 해당 요청을 처리하고, 권한을 가지고 있지 않다면, 에러코드를 보내게 된다.
