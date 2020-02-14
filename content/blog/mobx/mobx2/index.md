---
title: 'mobx store합치기'
date: '2020-01-23'
category: 'react'
---

# store 간 관계형성

## RootStore

- store폴더 안에 index.js를 만들어준다.
  ![image.png](https://images.velog.io/post-images/jotang/0905bf60-3d49-11ea-bef1-41521eb019a2/image.png)
- 만들어둔 store를 import해온다
- `this.store명 = new 새로운스토어(this)`
- 뒷부분의 this는 각 스토어들이 현재 RootStore가 무엇인지 알 수 있게 해준다.

- 각 스토어를 만들 때 RootStore를 파라미터로 넣어주었기 때문에 이를 따로 값으로 저장해야한다.

```jsx
//각 스토어에 추가한다.
constructor(root) {
	this.root = root;
}
```

### Provider에게 전달

- spread문법으로 props를 전달하게 되면, 자동으로 counter와 market스토어가 전달된다.
  ![image.png](https://images.velog.io/post-images/jotang/e3f2d270-3d49-11ea-988a-3703e1bfdc2e/image.png)

- market 스토어에서 counter 스토어에 접근하고 싶다면, `this.root.counter.number`로 접근하면 된다.

![image.png](https://images.velog.io/post-images/jotang/42503b50-3d4a-11ea-988a-3703e1bfdc2e/image.png)

- counter 스토어의 number의 기본값을 3으로 설정한다.

![image.png](https://images.velog.io/post-images/jotang/6ccd2780-3d4a-11ea-988a-3703e1bfdc2e/image.png)

- market 스토어에서 counter 스토어에 접근하여 number를 가져온다

```jsx
const { number } = this.root.counter;
```

- count를 counter스토어의 number로 지정한다.
- 상품을 클릭하게 되면 3개씩 선택되게 된다.

# mobX의 react component 최적화

> [velopert blog](https://velog.io/@velopert/MobX-3-%EC%8B%AC%ED%99%94%EC%A0%81%EC%9D%B8-%EC%82%AC%EC%9A%A9-%EB%B0%8F-%EC%B5%9C%EC%A0%81%ED%99%94-%EB%B0%A9%EB%B2%95-tnjltay61n)

## 1. 리스트를 랜더링 할 때에는, component에 리스트에 관련된 data만 props로 전달한다.

```jsx
@observer
class MyComponent extends Component {
  render() {
    const { todos, user } = this.props;
    return (
      <div>
        {user.name}
        <ul>
          {todos.map(todo => (
            <TodoView todo={todo} key={todo.id} />
          ))}
        </ul>
      </div>
    );
  }
}

// 비효율적인 코드
// user.name이 바뀔때도 component가 리랜더링 되기 때문
// 리스트를 따로 분리시키는 것이 좋다.
```

```jsx
@observer class MyComponent extends Component {
    render() {
        const {todos, user} = this.props;
        return (<div>
            {user.name}
            <TodosView todos={todos} />
        </div>)
    }
}

@observer class TodosView extends Component {
    render() {
        const {todos} = this.props;
        return <ul>
            {todos.map(todo => <TodoView todo={todo} key={todo.id} />)}
        </ul>)
    }
}
```

## 2. 세부참조(dereference)는 최대한 늦게하자

- 세부참조란, 특정 객체의 내부의 값을 조회하는 것을 말한다.

```jsx
const itemList = items.map(item => (
  <BasketItem
    name={item.name}
    price={item.price}
    count={item.count}
    key={item.name}
    onTake={onTake}
  />
));
// item에서 name, price, count를 조회하는 것이 세부참조

const itemList = items.map(item => (
  <BasketItem item={item} key={item.name} onTake={onTake} />
));
```

## 3. 함수는 미리 binding(바인딩)하고, 파라미터는 내부에서 넣어주기

- component에서 함수를 전달할 때는 미리 바인딩을 하는 것이 좋다.
- 유동적인 파라미터를 넣는 작업은 component 밖이 아니라 안에서 하는 것이 좋다.

```jsx
render() {
    return <MyWidget onClick={() => { alert('hi') }} />
}
// 좋지 않은 코드
render() {
    return <MyWidget onClick={this.handleClick} />
}

handleClick = () => {
    alert('hi')
}
```
