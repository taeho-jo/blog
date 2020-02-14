---
title: 'redux에서 mobx로'
date: '2020-01-24'
category: 'mobx'
---

# mobX를 적용해보자

##### 굉장히 간단한 예제임을 감안해주세요

##### 작성하는 방법이 많아 공부차원에서 작성하였으니 틀린 부분이 있다면 더 공부하도록 하겠습니다.

### store 만들기

![image.png](https://images.velog.io/post-images/jotang/02c13170-3db2-11ea-9648-e7ed9b3422f6/image.png)

- 먼저 store를 만들어준다.
- mobx의 observer를 import하여 관찰할 친구를 감싸준다.

```jsx
cosnt AddStore = observer({
	commentArr: [] //commentArr이라는 빈 배열을 전역으로 상태관리를 할 예정

    add(title, content) {     //add라는 action이다. title,content를 받아올 예정
  	this.commentArr.push({
      	id: id++,
      	title,
      	content
      })         //commentArr에 객체의 형태로 id와 받아온 title,content push한다.
	}
})
```

- 이렇게하면 store는 준비 끝

### store 불러오기

- store를 불러와 사용할 component 파일로 가서 import 한다.
  ![image.png](https://images.velog.io/post-images/jotang/3df13640-3db3-11ea-8c28-6fa26897aa6f/image.png)
- 이렇게 하면 준비 끝
  ![image.png](https://images.velog.io/post-images/jotang/7f5022e0-3db3-11ea-8b18-3b962df0709f/image.png)
- onSubmit 함수를 실행할 때, `AddStore.add(title, content)`를 하여 action도 발생시켜 준다.
- map함수를 이용하여 뿌릴때에도 `AddStore.commentArr`로 경로를 찾아들어가면 된다.
  ![image.png](https://images.velog.io/post-images/jotang/c80ee0c0-3db3-11ea-823e-5b6b4250944b/image.png)
- 내용이 보여지는 부분도 마찬가지.

## mobX..

- 엄청 간단한 예제로 사용을 해보았지만, 여러가지 사용법이 있다보니 헷갈리고, 또 맞게 사용하고 있는지에 대해 많은 고민이 들었다.
- store를 만들 때, decorator를 사용하여 class형으로 만드는 법도 있었고, 그냥 함수형으로 만드는 법도 있었다.
- component에서 사용할 때에도 useObserver / useLocalStore를 사용하는 방법, Provider로 합쳐서 사용하는방법, 여기에서도 store를 각각 import해서 사용할 수 도 있었고, RootStore를 만들어서 한번에 관리하는 방법도 있는 등 굉장히 많은 방법이 있었다.
- 아직 이제 시작하는 단계인 만큼 개념을 확실히 이해하고 넘어가도록 해야겠다.

## useObserver, useLocalStore

- useObserver는 mobX에서 제공하는 hook이라고 하는데, 추후에 공부하여 적용을 시켜보도록 하겠다.
- 해당 component에서만 관리할 state는 useState말고, mobX에서 제공하는 useLocalStore로 관리 할 수도 있다고 한다. 이 부분은 조금 더 공부해서 블로깅 하도록 하겠다.
