---
title: 'firebase Auth'
date: '2020-02-01'
category: 'firebase'
---

# firebase를 이용한 Login

## email, google 로그인

### firebase npm 설치

`npm install firebase`

### firebase 적용

- firebase.js 파일을 생성 한 후에 SDK를 등록한다.

```jsx
import firebase from 'firebase';
const config = {
  apiKey: '비~밀~',
  authDomain: '비~밀~',
  databaseURL: '비~밀~',
  projectId: '비~밀~',
  storageBucket: '비~밀~',
  messagingSenderId: '비~밀~',
  appId: '비~밀~',
  measurementId: '비~밀~',
};
try {
  firebase.app();
} catch (error) {
  firebase.initializeApp(config);
}
export default firebase;
```

- config는 git에 올라가거나 노출이 되면 좋지 않기때문에 다른 파일로 따로 관리하여 import하여 사용한다.
- config 폴더를 만들고 그 안에 firebase.js를 또 만들어 준다.

```jsx
import firebase from 'firebase';
import config from './config/firebase';
try {
  firebase.app();
} catch (error) {
  firebase.initializeApp(config);
}
export default firebase;
```

- 적용 끝! 이제 쓰기만 하면된다.

### google login

```jsx
login = () => {
  const provider = new firebase.auth.GoogleAuthProvider();
  firebase
    .auth()
    .signInWithPopup(provider)
    .then(res => {
      console.log(res.credential.accessToken);
      this.props.userInfo.userName = res.user.displayName;
      console.log(res.user);
      if (res) {
        sessionStorage.setItem('accessToken', res.credential.accessToken);
        this.moving();
      }
    })
    .catch(error => {
      alert('다시 로그인하세요' + error.message);
      console.log(error);
    });
};
```

- this.props.userInfo는 mobX store에서 관리하고 있는 친구이다.
- 받아온 토큰을 sessionStorage에 저장한다. 그리고 home 화면으로 이동
- popup창을 그냥 끄거나 로그인 실패시는 alert창이 뜬다.

### email login

```jsx
emailLogin = () => {
  firebase
    .auth()
    .signInWithEmailAndPassword(this.state.id, this.state.password)
    .then(res => {
      console.log(res.credential);
      console.log(res.user);
      if (res) {
        // sessionStorage.setItem("refreshToken", res.credential.refreshToken);
        this.props.userInfo.userName = res.user.email;
        this.moving();
      }
    })
    .catch(error => {
      alert('다시 로그인하세요');
      console.log(error);
    });
};
```

- 구글 로그인과 동일하다.
- signInWithEmailAndPassword method를 이용하고, 값으로 state에서 관리하는 id, password 값을 넣어준다.
- credential 에는 토큰 등의 정보가 담겨있다.
- user에는 user에 관련한 정보가 담겨있다.
- 가상 아이디다 보니 credential에는 아무런 정보가 없다.

### sign-up

```jsx
signup = () => {
  firebase
    .auth()
    .createUserWithEmailAndPassword(this.state.id, this.state.password)
    .then(res => {
      console.log(res);
      this.setState({
        id: '',
        password: '',
      });
    })
    .catch(error => {
      console.log(error);
    });
};
```

- 회원의 개인정보까지 받지는 않는 로직
- firebase로의 회원가입이 가능하고, 로그인도 가능하다.
