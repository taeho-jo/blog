---
title: 'justSell Project3'
date: '2020-01-18'
category: 'next.js'
---

# JustSell Project

## 1. Project 소개

### JustSell Project (상품 유통 운영 관리 서비스)

## 2. Skills

### next.js / hooks / redux / styled-components

## 3. 맡은 역할

### login page / consulting page

### nav page ( 공통 menu nav)

### report page

## 4. 잘 했던 점

- Login 시에 access_token을 sessionStorage에 저장하고, logout 시에는 삭제했던 점
- redux를 이용하여 언어전환 버튼을 클릭시에 언어전환을 구현한 점
- nav 를 공통으로 사용할 수 있게 한 점.
- next/router를 이용하여 page와 page를 연결한 점

## 5. Problems & Solution

### 5.1 Problems

1. **nav page 공통영역 설정**
   - nav 는 fixed 되어있고, 팀원들이 import해서 사용할 수 있도록 공통영역을 설정해줘야 했던 부분

2) **report page**
   - chart library 를 사용할 때, 적용하는 부분

### 5.2 Solution

1. **nav page**
   ![image.png](https://images.velog.io/post-images/jotang/1cdae9c0-397f-11ea-80e6-f78fc703e45d/image.png)

- nav를 import하여 children 영역에 page를 구성하고자 하였다.
- 왼쪽 menu nav의 크기만큼을 뺀다( `calc(100% - 240px)` )
- 처음에는 absolute로 위치를 잡았는데, 부모인 body가 자식이 있는지 모르는 문제가 발생하여서 margin과 padding으로 위치를 잡았다.

2. **report page**
   ![image.png](https://images.velog.io/post-images/jotang/8bb53580-397f-11ea-ae5a-5dc51127c734/image.png)

- chart library를 사용한 컴포넌트를 import해야하는 부모 컴포넌트에서 SSR을 하지 않겠다고 선언을 하면서 import해야한다.
- 이 부분은 SSR의 특징인데, 추후에 정리하여 블로깅 하도록 하겠습니다.

## 6. 기록하고싶은 codes

### getInitialProps

![image.png](https://images.velog.io/post-images/jotang/d5c5c7c0-397f-11ea-b7d2-475cfb8bf424/image.png)

- 랜더링이 되는 page에서, 화면에 랜더링이 될 때 부터 data가 필요하다면, componentDidMount나 useEffect를 사용하는 것이 아니라 getInitialProps를 사용하여 data를 가져와야한다.
- return을 통해서 저장을 하고, props로 전달 받아 사용한다

### useEffect

![image.png](https://images.velog.io/post-images/jotang/2b56d710-3980-11ea-b834-3d252f191958/image.png)

- 부모 컴포넌트에 포함되는 컴포넌트에서 data가 필요하다면 componentDidMount나 useEffect를 사용하면된다.
- useEffect의 두 번째 인자로는 빈 배열([])을 넣어주어야 무한루프를 피할 수 있다.

# 느낀 점

개인적으로는 아쉬운 부분이 많은 프로젝트였고, 새로 공부해야 하는 부분들이 많아서 힘들기도 한 프로젝트였다.
그럼에도 next.js / redux / styled-components 를 공부하고 알게 되어서 좋은 시간이었다. next.js같은 경우에는 아직 안다고 말 할 수는 없지만 맛을 한번 보았기 때문에 천천히 다시 공부를 한다면 이번에 사용한 것 보다 더 잘 이용할 수 있게 될 것이다. 또 redux도 로그인페이지에서만 사용하게 되었지만, 예제코드가 아니라 실제로 코드를 만들고 작성하면서 redux의 흐름에 대해서 확실히 알게 되었다. 개념을 조금 만 더 탄탄히 만든다면 다른 프로젝트에서도 사용이 충분히 가능할 것 같다. styled-components의 경우 처음에는 너무나 불편하고 이걸 왜 사용하는지 의문이 들었지만, 2주간 사용하다보니 정말 유용한 기능들이 많다는 것을 깨달을 수 있었다. 코드를 돌아보면서 부족한 부분들을 조금 더 보완하고 개념들을 채워나가야 겠다.

#### 추후에 계속해서 업로드가 될 수 있습니다.
