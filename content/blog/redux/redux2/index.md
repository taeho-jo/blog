---
title: 'Next.js Redux2'
date: '2020-01-19'
category: 'redux'
---

# Redux를 이용해 언어설정을 해보자

## 적용할 언어만 정리해둔 파일을 만든다.

- 새로운 폴더에 언어만 정리해둔 파일을 생성한다.

![image.png](https://images.velog.io/post-images/jotang/bed5bee0-3a82-11ea-bc2b-d3e4d0508d4e/image.png)

- 한국어를 정리한 KO를 만들고, page별로 구분한다.(login & admin)
  ![image.png](https://images.velog.io/post-images/jotang/d9ac2b00-3a82-11ea-bc2b-d3e4d0508d4e/image.png)
- 영어를 정리한 EN을 만들고, page별로 구분한다.(login & admin)

## action & reducer를 생성하자

![image.png](https://images.velog.io/post-images/jotang/45090fd0-3a83-11ea-a1ad-4fad434d4fa8/image.png)

- KO, EN을 import한다
- action type과 action 함수를 생성한다.

```jsx
export const CHANGE_ENGLISH = 'CHANGE_ENGLISH';
export const CHANGE_KOREAN = 'CHANGE_KOREAN';

export const changeEnglish = () => ({
  type: CHANGE_ENGLISH,
});
export const changeKorean = () => ({
  type: CHANGE_KOREAN,
});
```

```jsx
export const initialState = {
  lang: KO,
};
// state의 기본값으로 lang: KO를 설정해준다
```

```jsx
const reducer = (state = initialState, action) => {
  switch (action.type) {
    case CHANGE_ENGLISH:
      return {
        lang: EN,
      };
    case CHANGE_KOREAN:
      return {
        lang: KO,
      };
    default:
      return state;
  }
};
```

- reduer함수는 state와 action 두가지를 인자로 받는다.
- state = initialState로 초깃값으로 설정해준다.
- action.type이 CHANGE_ENGLISH라면 lang: EN으로, CHANGE_KOREAN이라면 lang: KO로 설정한다.

## dispatch로 action을 발생시키고 store와 연동을 해보자

![image.png](https://images.velog.io/post-images/jotang/ea958720-3a89-11ea-bc2b-d3e4d0508d4e/image.png)

- connect를 import해서 기존의 component인 AdminBox를 감싼다.
- 이 page에서는 action 함수를 발생시키지 않기 때문에 생략하였다.

```jsx
connect(
  mapStateToProps,
  matDispatchToProps
)(AdminBox);
// mapStateToProps : store의 state를 불러와 사용할 수 있게 해준다.
const mapStateToProps = state => {
  return { name: state.ChangeLanguage };
};
```

- name이라는 key에 담아주고 props로 전달해 사용하게 된다.

## action!!

![image.png](https://images.velog.io/post-images/jotang/d7cac730-3a8a-11ea-8978-5d3ee0e9c1b4/image.png)

```jsx
export default connect(
  null,
  { changeEnglish, changeKorean }
)(Language);
// mapStateToProps는 생략이 불가능하다. 따라서 사용하지는 않지만 null을 넣어준다.
// mapDispatchToProps에서 따로 dispatch를 사용하지 않고 바로 action 함수를 적어주고, 사용한다.
```

- action 함수를 import하고 props로 전달을 받은 후에 사용한다.

![image.png](https://images.velog.io/post-images/jotang/b2c4b610-3a8c-11ea-a923-71d0c816de13/image.png)

![image.png](https://images.velog.io/post-images/jotang/fb4f9d40-3a8d-11ea-8978-5d3ee0e9c1b4/image.png)
