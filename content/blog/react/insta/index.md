---
title: 'React_Instagram clone'
date: '2019-12-16'
category: 'react'
---

# 1. Problems & Solution

### 1. problems

- for문을 이용하여 comment 입력하였기에 삭제기능이 구현하기 힘들어졌다. 특정 버튼을 눌렀을 때 index 또는 id, key의 value를 알아 내기 힘들었다.

- findIndexOf() 등 많은 method를 이용해보았지만 효과가 없었다.
- Deep Copy, Shallow Copy 의 개념을 알지 못했다.

### 2. solution

1. map() 을 이용하였다.

```jsx
for (let i = 0; i < date.length; i++) {
  lists.push(
    <p key={date[i].id} className="js-p">
      <a herf={date[i].id}>{date[i].who}</a>
      <span className="js-span"> {date[i].content}</span>
      <button
        value={date[i].id}
        className="del"
        onClick={e => this.props.handleDelete(e)}
      >
        {date[i].cancel}
      </button>
    </p>
  );
}
```

```jsx
//변경후
let lists = date.map(data => (
        <p
          key={data.id}
          clasName="js-p">
          <a
            herf={data.id}>{data.who}
          </a>
          <span
            className="js-span"> {data.content}
          </span>
          <button
            value= {data.id}
            className='del'
            onClick={event => this.props.handleDelete(data.id) }>
            {data.cancel}
          </button>
        </p>
```

2. map()을 이용하였기에 id에 접근할 수 있게 되었고, filter() 를 사용하였다.

```jsx
handleDelete = (data) => {
      const Arr = this.state.Comment;
      const newArr = Arr.filter(e => (
        e.id !== data
      ))
      this.setState({
        Comment: newArr,
      })
```

3. Deep Copy 와 Shallow Copy 는 아래에서 알아보겠다.

# 2. Deep Copy & Shallow Copy

- Copy란 기존의 object와 같은 값을 가진 새로운 object를 만든다는 것이다.
- object의 가진 value 형식과 참조형식의 복제 방식에 따라 Deep Copy와 Shallow Copy의 개념이 나누어지게 된다.

### 2.1 Deep Copy

- object가 가진 모든 값과 참조형식을 복사하는 것을 말한다.
- 참조값의 복사가 아닌 참조된 객체 자체가 복사되는 것을 말한다.

![캡처.JPG](https://images.velog.io/post-images/jotang/e3810bc0-2027-11ea-a1ff-cfa4b91056fc/캡처.JPG)

- 복사본과 원본의 메모리 참조 공간 자체가 다르기 때문에 각자의 값을 참조하게 되고, 변경사항이 있더라도 서로 영향을 미치지 않는다.

### 2.2 Shallow Copy

- object가 가진 값을 새로운 object로 복사하는데 만약 참조타입이라면 참조값만 복사하게 된다.
- 복사본이라기 보다는 바로가기와 같은 느낌이다.

![캡처.JPG](https://images.velog.io/post-images/jotang/524fbec0-2028-11ea-a1ff-cfa4b91056fc/캡처.JPG)

- 같은 참조 값을 가지게 된다
- 수정사항이 생기게 되면 영향을 미칠 수 있다.
