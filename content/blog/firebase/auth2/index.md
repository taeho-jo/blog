---
title: 'firebase Auth2'
date: '2020-02-02'
category: 'firebase'
---

# facebook login

### facebook login

- facebook login 역시 google login과 다를 것은 별로 없다.
- facebook for developers 사이트에서 가입과 등록을 한 후에 firebase와 연동만 해주면된다.

```jsx
login = () => {
  const provider = new firebase.auth.FacebookAuthProvider();
  // provider 부분만 변경한 후 진행하면 된다.
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

# mobX로 상태 관리하기

##### 추후에 더 많은 상태관리를 할 수 있습니다.

##### 현재는 기능별 구조를 잡는 것에 집중하고 있습니다.

##### 다소 코드가 이쁘지 않을 수 있습니다.

### feature function을 하나의 파일로 관리하기 위해서 접근이 더 용이할 수 있도록 mobX역시 function으로 작성하였다.

```jsx
import { observable } from 'mobx';

const LoginUserInfo = observable({
  email: '',
  password: '',
  user: {
    userName: '',
  },

  changeInfo(email, password) {
    (this.email = email), (this.password = password);
  },

  welcome(text) {
    this.user = {
      userName: text,
    };
  },
});

export default LoginUserInfo;
```

# feature function import하여 사용하기

### view와 feature을 구분하여 작성하여야 추후에 유지보수에 용이하다고 한다.

### view page에서는 최소한의 function만 사용하고, feature function 은 하나의 파일로 만들어 관리하고, import하여 사용한다.

```jsx
import firebase from '../firebase';
import Router from 'next/router';
import LoginUserInfo from '../stores/loginStore/loginUser';

const moving = () => {
  Router.push('/');
};

export const googleLogin = () => {
  const provider = new firebase.auth.GoogleAuthProvider();
  firebase
    .auth()
    .signInWithPopup(provider)
    .then(res => {
      if (res) {
        LoginUserInfo.welcome(res.user.displayName);
        sessionStorage.setItem('accessToken', res.credential.accessToken);
        moving();
      }
    })
    .catch(error => {
      alert('다시 로그인하세요' + error.message);
      console.log(error);
    });
};

export const facebookLogin = () => {
  const provider = new firebase.auth.FacebookAuthProvider();
  firebase
    .auth()
    .signInWithPopup(provider)
    .then(res => {
      if (res) {
        LoginUserInfo.welcome(res.user.displayName);
        sessionStorage.setItem('accessToken', res.credential.accessToken);
        moving();
      }
    })
    .catch(error => {
      alert('다시 로그인하세요' + error.message);
      console.log(error);
    });
};

export const emailLogin = () => {
  firebase
    .auth()
    .signInWithEmailAndPassword(LoginUserInfo.email, LoginUserInfo.password)
    .then(res => {
      if (res) {
        LoginUserInfo.welcome(res.user.displayName);
        // sessionStorage.setItem('accessToken', res.credential.accessToken)
        moving();
      }
    })
    .catch(error => {
      alert('다시 로그인하세요');
      console.log(error);
    });
};

export const signup = () => {
  firebase
    .auth()
    .createUserWithEmailAndPassword(LoginUserInfo.email, LoginUserInfo.password)
    .then(res => {
      console.log(res);
      moving();
    })
    .catch(error => {
      alert('다시 가입고고');
      console.log(error);
    });
};

export const logout = () => {
  firebase.auth().signOut();
  window.sessionStorage.clear();
  LoginUserInfo.user.userName = '';
  moving();
};
```

- import하여 사용하는 page

![image.png](https://images.velog.io/post-images/jotang/e7f86330-45af-11ea-80bd-fb4dd3073590/image.png)
