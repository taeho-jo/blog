---
title: 'weHome Project'
date: '2020-01-04'
category: 'react'
---

# weHome Project

## 1. Project 소개

### 오늘의집 clone

## 2. Skills

### HTML / SASS / JavaScript / React

## 3. 맡은 역할

### 오늘의 스토리(blog) 상세페이지

### 오늘의 딜(store) 상세페이지

## 4. 잘 했던 점

- back과 연결 전 미리 어떤 정보를 받을지 어떤 형식으로 받을 지 생각하여 JSON형식의 MockData를 작성하였고, 처음부터 fetch를 통해서 작성한 정보를 받아와서 뿌릴 수 있는 code를 작성하였고, 나중에 back에서 data를 보내줄 때, fetch의 주소부분만 해당 API 주소로 바꾸면 되도록 하였다.
- 미리 MockData를 작성하여 준비하다 보니, back담당자와 의견을 주고 받을 때 대화하기가 좋았다.
- 실제 page와 거의 유사하도록 SASS통해 작업하였다.

## 5. Problems & Solution

### 5.1 Problems

1. **오늘의 딜(store)**

   1-1. 오늘의 딜 page에서 상품을 선택하는 select와 option을 map을 통해서 뿌리는 부분

   1-2. 상품 선택시 가격이 합산되는 부분

   1-3. 상품 선택 창이 2곳인데 연동을 해야하는 부분

2. **오늘의 스토리(blog)**

   2-1. 댓글창의 page가 넘어가는 부분(pagination)과 댓글을 다섯개만 추출하여 보여줘야하는 부분

### 5.2 Solution

1. **오늘의 딜(store)**

   1-1. JSON 형식으로 작성된 정보의 mockdata구조를 다시 작성하였고, option tag 부분을 따로 map을 하고, select tag 부분도 따로 map을 하였다.
   ![image.png](https://images.velog.io/post-images/jotang/4f281290-2ec6-11ea-bd25-89da550211f0/image.png)

   - select태그를 map하는 부분. `<DealSelector />` component는 option tag가 있는 컴포넌트이다. `data={el}` 을 통해 props로 전달하였다.
     ![image.png](https://images.velog.io/post-images/jotang/9a34b810-2ec6-11ea-bd25-89da550211f0/image.png)

   - props로 전달 받은 데이터를 통해 option tag의 map을 진행하였다.

1-2. onClick이벤트를 통해 해당 상품을 선택할 때, 가격의 정보만 따로 추출하여 배열로 만들었고, 배열에 추가된 가격정보는 선택된 것이기 때문에 합산하여 보여주었다. 해당 상품의 선택을 취소할 때에는 filter함수를 통해 해당 배열에서 제거하고 새로운 배열로 만들었다.
![image.png](https://images.velog.io/post-images/jotang/ff0f9360-2ec4-11ea-ba4e-4b3957f898f9/image.png)

- parseFloat 함수를 이용하여 string이였던 가격정보를 바로 number로 변환하여 배열에 추가하였다.
  ![image.png](https://images.velog.io/post-images/jotang/85647610-2ec5-11ea-ba4e-4b3957f898f9/image.png)
- 배열에서 삭제하는 부분

1-3. React Redux를 이용하여 전역에서 state를 관리해야 구현할 수 있는 부분인거 같았고, 아직 Redux를 학습하지 않았기 때문에 결국 구현하지 못했다. 2차프로젝트 시작 전 Redux에 대해서 학습을 미리 진행하여, 2차 프로젝트에서 Redux를 사용해 볼 것이다.

2. **오늘의 스토리(blog)**

   2-1. pagination
   ![image.png](https://images.velog.io/post-images/jotang/c960b7f0-2ec7-11ea-8594-171ac99cfc62/image.png)

   - post할 댓글의 갯수를 state에서 미리 정한 후, props로 전달받은 data 배열의 길이 나누기 state에서 미리 정의해 둔 postpage를 한다. 소수점으로 나오게 되는데 이 부분을 무조건 올림을 할 수 있도록 Math.ceil를 이용하였다. i를 새로운 배열에 넣어준다. data 배열안의 요소가 5개 까지 일 때는 i = 1 이되고, 6이 되는 순간 i = 2가 되게된다. 이 새로운배열 pageNum을 map함수를 이용하여 뿌려준다.

     ![image.png](https://images.velog.io/post-images/jotang/d38b7390-2ec8-11ea-8594-171ac99cfc62/image.png)

   - 배열에서 5개만 추출하여하는데 그 인덱스 번호를 (currentPage - 1) _ 5 부터 (currentPage - 1) _ 5 + 5 까지로 하고 slice한다.

## 6. 기록하고싶은 codes

### fetch함수

```javascript
const fetchAPI = (...req) =>
  fetch(...req)
    .then(res => res.json())
    .catch(err => {
      console.log(err);
      throw err; //에러를 밖으로 던짐 갖고있음 안되니까
    });

export default fetchAPI;
```

- 기본 fetch함수를 정의해 두고 import해서 사용한다.

![image.png](https://images.velog.io/post-images/jotang/98a5d350-2ec9-11ea-90e1-891046f5b5ca/image.png)

#### 추후에 계속해서 업로드가 될 수 있습니다.
