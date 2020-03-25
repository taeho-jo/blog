---
title: 'Vue TodoList'
date: '2020-03-25'
category: 'Vue'
---

# vue 설정에 대해 다시

```
npm install -g @vue/cli

vue create 폴더명
```

`vue create 폴더명` 명령어를 실행하면 아래 스크린샷 처럼 나오게 된다.

![](https://images.velog.io/images/jotang/post/8284fe75-c2f1-4cf6-9ea5-2bb1ce96ffd5/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-03-25%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.33.15.png)

`defalut` 는 기본적으로 설정되어 있는데로 설치하겠다는 것.
`Manually select features` 는 직접 선택할 수 있다.

![](https://images.velog.io/images/jotang/post/35dd4880-434b-4ac5-8c7a-a5e0ba69daf4/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-03-25%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.36.52.png)

TypeScript, Vuex, Router 등 여러가지를 선택할 수 있다.
일단 저 상태 그대로 진행하도록 한다.

![](https://images.velog.io/images/jotang/post/f21eac49-d9d7-4154-a5eb-69a1f9393f2d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-03-25%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.38.24.png)

ESlint와 Prettier를 선택한다.

![](https://images.velog.io/images/jotang/post/487db67a-3c0a-409d-8bdd-997fa37ca396/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-03-25%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.39.12.png)
![](https://images.velog.io/images/jotang/post/8f9d8546-23ba-409c-a629-e6ec28cf4af9/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-03-25%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.39.48.png)

package.json파일에 위치할 필요가 없기때문에 그대로 진행하면 된다.

![](https://images.velog.io/images/jotang/post/23250cfe-dcbf-4a98-89e7-616fac3824e5/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-03-25%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.40.47.png)

이 설정을 저장할 것이냐고 묻는 것인데 yes or no 선택하면된다

# eslint & prettier

##### 확실하지가 않습니다..vue를 더 경험해 나가면서 자세히 알아보도록 하겠습니다.

먼저 최상위에 `.eslintrc.js`파일 만들고 아래의 코드를 작성한다.

```js
module.exports = {
  root: true,
  env: {
    node: true,
  },
  extends: ['plugin:vue/essential', 'eslint:recommended', '@vue/prettier'],
  parserOptions: {
    parser: 'babel-eslint',
  },
  rules: {
    'no-console': process.env.NODE_ENV === 'production' ? 'error' : 'off',
    'no-debugger': process.env.NODE_ENV === 'production' ? 'error' : 'off',
  },
};
```

# TodoList

어딜가도 빠지지 않는 TodoList를 만들면서 실습해보려고 한다.

오늘 만드는 TodoList는 모바일 view를 기준으로 만들었다.
완성된 모습은 아래의 이미지와 같다.
![](https://images.velog.io/images/jotang/post/f84581df-89b3-4cb1-82b6-d75b7f13e505/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-03-25%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%204.47.32.png)

##### css부분은 필요한 부분만 보도록 하겠다.

### App Component

먼저 최상위 컴포넌트는 `App component`.
그 안에 TodoHeader, TodoInput, TodoList, TodoFooter 4개의 자식컴포넌트가 있다.

```html
<template>
  <div>
    <TodoHeader />
    <TodoInput v-on:addTodo="addTodo" />
    <TodoList v-bind:propsData="todoItems" v-on:removeTodo="removeTodo" />
    <TodoFooter v-on:removeAll="clearAll" />
  </div>
</template>
```

- TodoInput 컴포넌트에서 추가 버튼을 클릭했을 때, App 컴포넌트로 이벤트를 전달할 수 있게 v-on 디렉티브를 추가하였다. `($emit으로 addTodo가 왔을 경우 addTodo함수 실행)`

- TodoList 컴포넌트에는 props로 todoItems를 전달해주기 위해서 v-bind를 사용하였고, 삭제 버튼을 클릭했을 때, 삭제하기 위해서 v-on 디렉티브도 추가하였다.`($emit으로 removeTodo가 왔을 경우 removeTodo함수 실행)`

- TodoFooter 컴포넌트에서 전부 삭제 버튼을 클릭했을 때, 이벤트가 전달될 수 있도록 v-on 디렉티브를 추가하였다.`($emit으로 removeAll이 왔을 경우 removeAll함수 실행)`

```js
<script>
// 컴포넌트들을 import한다.
import TodoHeader from "./components/TodoHeader";
import TodoInput from "./components/TodoInput";
import TodoList from "./components/TodoList";
import TodoFooter from "./components/TodoFooter";

export default {
  data() {
    return {
      todoItems: []
    };
  },
  methods: {
    addTodo(data) {
      localStorage.setItem(data, data);
      this.todoItems.push(data);
    },
    clearAll() {
      localStorage.clear();
      this.todoItems = [];
    },
    removeTodo(data, index) {
      localStorage.removeItem(data);
      this.todoItems.splice(index, 1);
    }
  },
  created() {
    if (localStorage.length > 0) {
      for (let i = 0; i < localStorage.length; i++) {
        this.todoItems.push(localStorage.key(i));
        console.log(localStorage.key(i));
      }
    }
  },
  components: {
    TodoHeader,
    TodoInput,
    TodoList,
    TodoFooter
  }
};
</script>
```

- 먼저 todoItems를 빈 배열로 정의하여준다. (처음 아무것도 등록되어 있지 않은 상태)

- **addTodo** : 인자로 data를 받아온다. 로컬스토리지에 key와 value를 받아온 인자인 data로 한다. 그리고 빈 배열이였던 todoItems에 push 한다.

- **removeTodo** : 인자로 data, index 두 가지를 받는다. 받아온 인자인 data는 로컬스토리지에 저장되어 있는 key와 동일하기 때문에, 로컬스토리지에서 먼저 삭제한다. 그리고 todoItems 배열에서 두번째 인자로 받아온 index부터 1개를 splice한다.

- **removeAll** : 로컬스토리지에 저장되어 있는 목록을 전부 삭제하고, todoItems 배열도 빈 배열로 바꾼다.

- **created()** : 라이프사이클 훅을 이용하여, 할 일이 이미 등록되어있는 경우에 로컬스토리지에서 data를 가져오게 한다.

### TodoHeader Component는 생략하도록 하겠습니다.

### TodoInput Component

```html
<template>
  <div class="inputBox shadow">
    <input
      type="text"
      v-model="newTodoItem"
      v-on:keyup.enter="addTodo"
      placeholder="what do you have to do?"
    />
    <button v-on:click="addTodo">추가</button>
  </div>
</template>
```

- v-model을 이용하여 text를 입력했을 때, vue 인스턴스에서 값을 인식할 수 있도록 한다.

- 버튼을 클릭했을 경우와 작성 후 enter를 누를 수도 있기에 두 곳에 v-on 디렉티브를 설정하였다.

```js
<script>
export default {
  data() {
    return {
      newTodoItem: ""
    };
  },
  methods: {
    addTodo() {
      if (this.newTodoItem !== "") {
        let value = this.newTodoItem && this.newTodoItem.trim();
        this.$emit("addTodo", value);
        this.clearInput();
      } else {
        alert("내용을 입력하세요");
      }
    },
    clearInput() {
      this.newTodoItem = "";
    }
  }
};
</script>
```

- input 창에 text를 입력하게 되면, newTodoItem의 값이 입력되는 순간마다 갱신된다.

- **addTodo** : input창에 아무것도 입력이 안되어있을 경우에는 alert창을 띄우게 되고, text를 입력한 후에 클릭 또는 엔터키를 누르게되면, value라는 변수에 input창에 입력되어 있는 text를 담는다.
  그리고 \$emit을 이용하여 부모 컴포넌트인 App component에 전달하게 된다. 그리고는 input창을 비우는 clearInput함수를 실행시킨다.

- addTodo함수에 `this.newTodoItem = '';`라고 직접 작성할 수 도 있지만 그렇게 하지 않은 이유는 `단일 책임 원칙(Single Responsibility Principle)` 때문이다.

단일 책임 원칙이란, 하나의 함수는 하나의 기능만을 담당하도록 설계하는 객체 지향 프로그래밍의 디자인 패턴이다.

### TodoList Component

```html
<template>
  <section>
    <transition-group name="list" tag="ul">
      <li v-for="(item, index) in propsData" v-bind:key="item">
        {{ item }}
        <span class="btn" type="button" @click="removeTodo(item, index)"
          >삭제</span
        >
      </li>
    </transition-group>
  </section>
</template>
```

- transition-group에 대해서는 밑에서 자세히 알아보도록 하겠다.

- `v-for` 디렉티브를 이용하여 데이터를 렌더링 한다.
  v-for를 사용할 때에는 key는 필수로 bind 해주어야한다.
  `(item, index) in propsData` props로 받아온 propsData가 원본 data이다.

- 삭제 버튼을 클릭하게 되면 click 이벤트가 발생하는데 v-on의 축약인 @를 사용하였다. 이벤트의 동작은 밑에서 알아보겠다.

```js
<script>
export default {
  props: ["propsData"],
  methods: {
    removeTodo(item, index) {
      this.$emit("removeTodo", item, index);
    }
  }
};
</script>
```

- 먼저 props를 정의해준다.

- **removeTodo** : 삭제 버튼이 클릭되면, 이벤트가 발생한다. 해당 item과 index를 담아서 부모 컴포넌트인 App component에 전달한다.

### TodoFooter Component

```html
<template>
  <div>
    <span class="allBtn" v-on:click="clearTodos">Clear ALL</span>
  </div>
</template>
```

```js
<script>
export default {
  methods: {
    clearTodos() {
      this.$emit("removeAll");
    }
  }
};
</script>
```

- 버튼이 클릭되면 clearTodos 이벤트가 발생하고, 부모 컴포넌트인 App component에게 소식을 전해준다.

### Vue Animation

vue 애니메이션은 vue 프레임워크 자체에서 지원하는 애니메이션 기능이다.

데이터 추가, 변경, 삭제에 대해서 페이드인, 페이드 아웃 등의 여러 가지 에니메이션 효과를 지원한다.

간단한 데이터부터 목록 아이템까지 모두 지원할 뿐만 아니라, 기타 자바스크립트 애니메이션 라이브러리나 CSS애니메이션 라이브러리도 함께 사용할 수 있다.

> 자세한 내용은 vue animation 공식 문서를 참고.
