---
title: 'AWS ( Amazon Web Services )'
date: '2020-01-03'
category: 'web'
---

![image.png](https://images.velog.io/post-images/jotang/9b22aa40-2eb5-11ea-aaa7-6f7e5296894f/image.png)

# 배포

- 현재 작업물은 그 파일이 들어있는 컴퓨터에서만 확인할 수 있는데, 그 결과물들을 누구나 와서 볼 수 있게하는 것 (내컴퓨터의 소스코드(남들은 볼 수 없음) => npm run start를 이용하여 화면 확인(localhost))

- 배포하기 위해서 AWS(Amazon Web Services)를 사용하게 된다.
- ip의 주소는 어디에서 접속하고 어떤 와이파이나 LAN을 이용하느냐에 따라 매일 바뀌게 된다. 배포를 위해서는 일정한 ip주소가 필요하게 되는데, 내 컴퓨터로 하기 위해서는 컴퓨터를 끄지말고 계속 켜둬야한다.(일년이라면 365일 24시간 계속해서 켜둬야한다.)

# AWS(Amazon Web Serivces)

- 결국 배포를 위해서는 주소를 사서 내 아이피랑 연결해서 보여주어야한다. AWS에서 가상서버 일부분을 빌려서 쓰게 되고, 소스코드,DB 등을 그 빌린 가상 서버 가상 공간에 올려두게 된다.

# 명령어 모음

## chmod -R 400

권한을 바꿈.
읽기만 가능. 왜 굉장히 중요한 파일이기 때문에 읽기권한만 준다. 수정불가능

![chmod -R 400.JPG](https://images.velog.io/post-images/jotang/4058b790-2d24-11ea-b09c-178f559dc60f/chmod-R-400.JPG)

# ssh -i [keyname.pem] ubuntu@[ip주소(0.0.0.0)]

- 우분투 환경으로 가는 명령어.
- "나 열쇠도 있고 주소도 있어. 확인해보고 나 보내줘"의 명령어

![ip주소.JPG](https://images.velog.io/post-images/jotang/ec1d57c0-2d24-11ea-b55a-a726513626b2/ip주소.JPG)

- ip주소는 AWS에서 확인 할 수 있다.
  ![우분투로 고고.JPG](https://images.velog.io/post-images/jotang/ef225a60-2d24-11ea-9945-17529f7ef015/우분투로-고고.JPG)

## npm과 node 설치 및 업데이트

### `curl -sL https://deb.nodesource.com/setup_10.x | sudo bash -`

### `sudo apt-get update`

### `sudo apt-get install nodejs`

### `npm -v (version확인)`

### `node -v (version확인)`

## git clone --> 폴더이동 --> npm install

- npm start는 개발모드

### build

- js파일 css파일이 모두모두 합쳐짐 js파일 하나 css파일 하나 html파일 하나로 만들어야한다

## npm run build

- build 파일이 생기고 이게 정말 필요한 각각 하나의 합쳐진 파일이 들어있게 된다.( js css html 각 하나씩)

```javascript
const express = require('express');
const path = require('path');
const app = express();
​
app.get('/ping', function(req, res) {
  res.send('pong');
});
​
app.use('/', express.static(path.join(__dirname, 'build')));
​
app.get('/*', function (req, res) {
  res.set({
    "Cache-Control": "no-cache, no-store, must-revalidate",
    "Pragma": "no-cache",
    "Date": new Date()
  });
​
  res.sendFile(path.join(__dirname, 'build', 'index.html'));
});
​
app.listen(80, () => {
  console.log('success!');
})
```

(로컬에서 ping을 요청주면 pong을 응답으로)

## npm install express

## vim server.js 입력모드로가려면 i

## [esc] -> :(나뭐할꺼야~) -> w(저장) -> q(종료) 하면 나가w진다.

## node server.js

## success를 확인하고 나서 -> 인스턴스 ip주소 확인해서 열면 사이트에 들어가지게 된다.
