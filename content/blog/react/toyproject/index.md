---
title: '댓글같은 게시판 만들기'
date: '2020-01-21'
category: 'react'
---

# Toy Project

## 댓글같은 게시판 만들기

### 목표

### 1. title(제목)과 content(본 내용)을 입력하면 밑에 목록이 생기고, 목록을 클릭할 시에 해당 내용으로 page이동이 일어난다.

### 2. redux를 이용하여 만들어 본다.

### 3. redux -> mobX로 바꾸어 적용해본다.

### 4. firebase를 이용하여 데이터를 추가하고 받아본다.

- **1월 21일 2단계까지 진행**

- CRA를 사용하고, styled-component, redux, mobx, firebase를 사용할 예정

### 제일 첫 화면(디자인은..신경쓰지 말아주세요)

![image.png](https://images.velog.io/post-images/jotang/bb7151b0-3c44-11ea-9119-39c2819698f6/image.png)

- 제일 첫 화면은 빈 화면
- 제목을 입력하고, 내용을 적은 후에 제출 button을 클릭하면 내용이 추가된다.
- 이 부분에서 굳이 사용하지 않아도 괜찮았겠지만 redux를 사용하였다.

![게시판 1.JPG](https://images.velog.io/post-images/jotang/a704efb0-3c45-11ea-9119-39c2819698f6/게시판-1.JPG)

![image.png](https://images.velog.io/post-images/jotang/9cf344e0-3c45-11ea-8ffc-814f6f5e3208/image.png)

![image.png](https://images.velog.io/post-images/jotang/06b0aa80-3c46-11ea-8ffc-814f6f5e3208/image.png)

- action은 ADD_COMMENTS라고 정의한다.
- action creator를 만드는데 comment를 인자로 받아온다.

```jsx
export const addComments = comment => {
  return {
    type: ADD_COMMENTS,
    payload: {
      id: id++,
      ...comment,
    },
  };
};
```

- 마지막으로 reducer 함수를 정의해 준다.
- 인자로 state와 action 두 가지를 받는데, state의 기본값은 빈 배열로 해준다.
- action의 타입이 ADD_COMMENTS라면 기존의 state를 가져오고, action의 payload를 넣어준다.

- submit event가 발생할 때, import해온 action creator를 dispatch한다.
- 이 때 title, content를 object로 만들어준다.
- 이 object는 reducer에 전달되고 action.payload로 들어가게 된다.
  ![image.png](https://images.velog.io/post-images/jotang/eaeefe90-3c46-11ea-9f10-23326a2d3e90/image.png)

- 내용을 입력하고 그것을 담아내는 것은 useState를 이용하였다.

![image.png](https://images.velog.io/post-images/jotang/a8ef6fb0-3c47-11ea-9119-39c2819698f6/image.png)

- mapStateToProps를 이용하여 store의 state를 가져왔고, 그것을 map()을 이용하여 랜더링해준다.
- 처음 store의 state는 빈 배열이라서 랜더링 되는 것이 없지만, action이 발생하는 순간 배열에 보내준 객체가 추가되고, map()이 작동하여 랜더링 되게 된다.
- onClick에 `{() => movePage(el.id}` 는 history.push를 위하여 사용하였다.

![image.png](https://images.velog.io/post-images/jotang/3be64500-3c48-11ea-9119-39c2819698f6/image.png)

![image.png](https://images.velog.io/post-images/jotang/86b51e30-3c48-11ea-9f10-23326a2d3e90/image.png)

- 해당 게시물을 클릭하게 되면 해당 게시글에 작성했던 제목과 내용이 나오는 page로 이동하게 된다.(Router의 history를 이용, 이전 블로그를 참조하세요)

- 역시 mapStateToProps를 이용하여 store의 state를 가져온다.
- state가 배열이기 때문에 index번호가 필요하고, 이 부분은 받아온 id - 1을 통해 해결한다.

#### 리덕스를 이용하여 게시판만들기는 완성 된 듯 하다. 내일은 mobX를 공부하여, 간단하게 나마 사용법을 익힐 수 있도록 하겠다.
