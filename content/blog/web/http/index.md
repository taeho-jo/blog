---
title: 'HTTP에 대해서'
date: '2019-12-10'
category: 'web'
---

# HTTP

### HTTP(hyperText Transfer Protocol)

- 팀 버너리스가 만든 3가지
- WWW(World Wide Web), URL, HTTP
- HTML 문서를 교환하기 위해 만들어진 Protocol(통신규약)

* HTML을 교환할 때 어떻게 전송할 건지에 대한 약속, 규약이라고 할 수 있다.

- HTTP가 필요한 이유는 각 나라마다 다른 언어이기 때문에 같은 정보라도 다르게 해석될 수 있기 때문이다. 따라서 서로 이해할 수 있는 규약을 통해서 보내야한다.(data를 보낸다는 것은 실제로 Text가 보내지는 것이라고 볼 수 있다.)

# HTTP의 2가지 속성

### 1. 요청과 응답(request & response)

- 컴퓨터 A가 컴퓨터 B에게 HTTP 요청을 보내고 그 요청을 받은 컴퓨터 B가 그 요청에 대한 응답을 response를 보내는 것이 하나의 HTTP통신이다.
- HTTP는 요청을 보내고 응답이 와야한다. 응답이 오지 않는 것은 error이다.
- 요청을 보내는 주체는 클라이언트, 일반적으로 사용자가 이용하는 웹 브라우저이고 응답을 보내는 주체는 Backend API이다.
- Frontend가 웹브라우저에게 HTML,CSS,JS등을 보낸다. 웹브라우저가 이걸 받아서 랜더링을하고 랜더링을 한 사이트가 우리가 보는 사이트이다. 우리가 검색을 하게 되면 웹브라우저가 Backend에 HTTP요청을 보낸다. 그 응답으로 Backend에서 검색에 대한 정보를 보내주게 된다. Frontend가 직접 보내는 것이 아니다.(Front - Web - Back 삼각관계)

### 2. Stateless(상태가 없다)

- 각각의 통신은 자기 자신의 통신만 알고 다른 통신에 대해서는 알지 못한다.
- 각 통신은 독립적이다.

```
<-------------------> 1
<-------------------> 2
<-------------------> 3
<-------------------> 4
//1번은 2,3,4번이 있는지 모르고 다른 통신도 똑같이 자기자신만 안다.
```

- 따라서 HTTP는 이전의 통신에 대한 정보를 담아서 보내고, 전송해야한다.( 통신을 처리하기 위해서 필요한 정보를 모두 다 담아서 같이 보내야한다.)
- 받은 정보를 저장하기 위해서 사용되는 것이 cookies, Session 등이 있다.
- 정보는 token의 형태로 보낸다(Token은 문자의 배열)

### 3. 응답과 요청의 구조

#### 3.1 Request

1. start line - 말 그대로 첫 줄. 정보를 담아 보낸다.

- HTTP Method : 해당 request가 의도한 action을 정의하는 부분
  - GET, POST, PUT, DELETE, OPTIONS 등이 있다.
  - 주로 GET과 POST가 쓰인다.
- request target
  - 해당 request가 전송되는 목표 url
  - 예를 들어 `/login`, `/search`, `/buy`
  - 자세한 주소와 같다.
- HTTP Version
  - 말 그대로 사용되는 HTTP의 버전
- End point : 끝 지점. 서로 바라보는 끝 지점. 서버와 서버가 있을 때 서로가 각각의 End가 된다.

```
GET /search HTTP/1.1
```

2. headers

- 해당 request에 대한 정보를 담고 있는 부분이다.(meta data)
- key와 value로 이루어져있다.
  - key : value
  - HOST: google.com => Key = HOST, Value = google.com
- Headers도 3부분으로 나뉜다
  - general headers, request headers, entity headers
- 자주 사용되는 header의 정보
  - HOST : 요청이 전송되는 target의 url(google.com)
    - **HOST Header + target = 완벽한 주소(naver.com/search)**
  - User-Agent : 요청을 보내는 클라이언트의 정보.(웹브라우저에 대한 정보, 크롬이면 크롬), 어떤 기종에서 사용하는 웹 브라우저인지도 수집하게 된다. 이러한 정보를 수집하여 맞춤형 광고에 사용된다.
  - Accept : 해당 요청이 받을 수 있는 응답의 타입
  - Connection : 해당 요청이 끝난 후에도 클라이언트와 서버가 계속해서 connection을 유지 할 것 인지 아닌지에 대해 지시하는 부분
  - Content-Type : 해당 요청이 보내는 메시지 body 의 타입. 예를 들어 json을 보낸다고 하면 `application/json`
  - Content-Length : 메세지 body의 길이

```
    Accept: */*
Accept-Encoding: gzip, deflate
Connection: keep-alive
Content-Type: application/json
Content-Length: 257
Host: google.com
User-Agent: HTTPie/0.9.3
```

3. body

- 해당 request의 실제 메세지이다. 이 메세지는 JSON의 형태로 주고 받는다.
- body가 없는 request도 많은데 GET request들은 대부분 body가 없다.

### 3.2 HTTP Respons의 구조

1. Status line

- 응답의 상태를 간략하게 나타내주는 부분이다.
- HTTP version, status code, status text

```
HTTP/1.1 404 Not Found
```

- status code
  1.  200 OK : 문제없이 실행 되었을 때.
  2.  301 Moved Permanently: url이 바뀌었다.
  3.  400 Bad Request : 요청이 잘못되었다
  4.  401 Unauthorized : 해당 요청을 위해서 로그인 또는 회원가입등이 먼저 필요하다는 것을 나타내려 할 때 쓰이는 코드
  5.  403 Forbidden : 해당 요청에 대한 권한이 없다
  6.  404 Not Found : 요청된 url이 존재 하지 않는다.
  7.  500 Intermal Server Error : 서버에 에러가 났을 때.

2. headers

- request의 header와 똑같은 개념

3. body

- request의 body와 일반적으로 동일하다.
- data가 포함되는 부분이다.
- data를 전송할 필요가 없을 경우 body는 비어있게 된다.

### HTTP Methods

1.  GET

- 어떠한 data를 server로 받아 올 때 사용한다.
- 생성, 수정, 삭제 없이 받아오기만 할 때 사용
- 가장 간단하고 많이 사용되는 method
- data를 받아올 때 사용되기 때문에 body를 안 보내는 경우가 많다.

2. POST

- 데이터를 생성, 수정, 삭제 할 때 주로 사용되는 method

3. PUT

- POST와 비슷하지만, data를 생성할 때만 사용되는 method이다. 실제로는 POST가 더 많이 쓰인다.

4. DELETE

- 특정 data를 server에 삭제 요청을 보낼 때 쓰인다. PUT과 마찬가지로 POST에가 더 많이 쓰인다.

### RESTful API

- endpoint 주소를 결정하는 패턴이다.

```
user: id/name/age - new user생성 : /POST/user
                      - 특정 user 정보 읽음 : /GET/user/1
                      - 모든 user 정보 읽음 : /GET/users
                      - 특정 user 정보 지움 : /DELETE/user/1
 HTTP method: /동사사용 X, 명사만/ 특정 value
```
