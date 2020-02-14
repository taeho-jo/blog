---
title: 'firebase Auth3'
date: '2020-02-07'
category: 'firebase'
---

# 익명사용자

- 홈페이지에 사용자가 접속하면, firebase의 currentUser를 조회하게 되고 그 값이 null이라면 익명사용자로 가입을 시키게된다. 이때 uid가 발급된다.
- 익명사용자로써 홈페이지를 이용을하고, 추후에 이메일, 구글 또는 페이스북으로 회원가입을 하게 되면 새로운 uid가 발급되는 것이 아니라 익명사용자로 가입되어 발급된 uid에 이메일, 구글, 페이스북이 연결된다.
- 만약 이미 회원가입이 되어있는 사용자라면, 기존의 로그인 방식으로 로그인 하면 된다.

# 익명사용자로 가입시키기

- 비회원로그인과 비슷한 것이다.
- 비회원로그인을 클릭하여 가입을 하게 되지만, 홈페이지 접속시에 바로 가입을 시킨다.

```jsx
// 최상위인 _app.js에서 실행
componentDidMount() {
	unKnownUser()
}
// unKnownUser함수
export const unKonwnUser = () => {
  if (Auth.currentUser === null) {
    Auth.signInAnonymously()
      .then(res => {
        if (res) {
          Auth.onAuthStateChanged(user => {
            console.log("익명유저", user);
            sessionStorage.setItem("anonymousUid", user.uid);
            sessionStorage.setItem("isAnonymous", user.isAnonymous);
          });
        }
      })
      .catch(error => {
        console.log(error);
      });
  }
};
```

- `signInAnonymously` method를 이용하여 가입시킨다.
- Auth는 `firebase.auth()` 이다.
- `onAuthStateChanged` 를 통해서 user의 정보를 가져온다.

# email 연결

- 익명사용자를 영구계정으로 연결시키기 위해서는 `linkWithCredential` method를 이용한다.

```jsx
export const linkEmail = (email, password) => {
  const credential = firebase.auth.EmailAuthProvider.credential(
    email,
    password
  );
  Auth.currentUser
    .linkWithCredential(credential)
    .then(res => {
      if (res) {
        console.log('이것은 연동1', res);
        Auth.onAuthStateChanged(user => {
          console.log('유저', user);
          sessionStorage.setItem('email', user.email);
          sessionStorage.setItem('uid', user.uid);
          sessionStorage.setItem('isAnonymous', user.isAnonymous);
          AuthStore.emailLoginUser(user.email, user.uid, user.isAnonymous);
        });
        AuthStore.emailLoginUser();
        moveHome();
      }
    })
    .catch(e => {
      console.log(e);
    });
};
```

- 사용자가 입력하는 email과 password를 넘겨준다.

# google & facebook 계정과 연결

- email과 연결시킬 때와는 달리 `linkWithPopup` method를 사용한다.

```jsx
export const linkGoogleSignUp = () => {
  const provider = new firebase.auth.GoogleAuthProvider();
  Auth.currentUser
    .linkWithPopup(provider)
    .then(res => {
      console.log('성공', res);
      sessionStorage.setItem('accessToken', res.credential.accessToken);
      sessionStorage.setItem('idToken', res.credential.idToken);
      sessionStorage.setItem('uid', res.user.uid);
      sessionStorage.setItem('email', res.user.email);
      AuthStore.googleUser(res.user.email, res.user.uid, res.user.displayName);
      AuthStore.isLogined();
      moveHome();
    })
    .catch(e => {
      console.log('실패', e);
    });
};
```

# sessionStorage 접근하기

- next.js의 서버에는 localStorage, sessionStorage라는 개념 자체가 없다.
- mount가 된 이후에 라이프사이클 method를 통해서 접근한다.
- componentDidMount method를 이용한다

```jsx
// _app.js에서 실행한다.
 componentDidMount() {
    CountryCodeStore.getGeoInfo();
    unKonwnUser();
    const token = sessionStorage.getItem("idToken") || "";
    if (token) {
      AuthStore.isLogined();
    } else {
      AuthStore.isLogouted();
    }
  }
```
