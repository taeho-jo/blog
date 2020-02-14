---
title: 'React의 시작'
date: '2019-12-08'
category: 'react'
---

# 1. JSX

### 1.1 JSX의 기본 규칙

- JSX element
- HTML 문법을 JS코드 내부에 써주는 그것이 바로 JSX이다.
- JS파일 어디에서나 사용할 수 있다.
- 변수에 저장할 수 있고, 함수의 인자로 넘길 수 있다.

- JSX attribute
  - attribute를 주고 싶을 때는 `""`을 사용해야한다.
  - HTML의 attribute name 과 다를 수있다.
    - class => className
  - `<a>` , `< img >` 와 같은 self-closing tag 역시 닫힘 Tag가 꼭 있어야한다.
    - `<div />, <div> </div> , <img />, <img> </img>`
  - Nested JSX
    - 소괄호 `()`로 감싸기, 중첩된 요소를 만들려면 `()` 로 감싸야한다.
    - 형제요소(Sibling)라면 무조건 감싸야하고, 최상위 Tag는 하나여야 한다.

```javascript
const good = (     		// 하나라도 감싸도 된다.
	<div>
    	<p>hi<p>
    <div>
 )

 const right = (   		//sibling 은 무조건 감싸야하고 최상위태그는 하나여야한다.
  	<div>
  		<p>list1<p>
  		<p>list2<p>
  	<div>

 const wrong = (		//감싸지 않아서 땡!
  	<p>list1<p>
  	<p>list2<p>
```

# 2. Component

- Component 란 재사용이 가능한 UI 단위를 말한다.
- Component를 하나만 만들어 재사용하고, 디자인 변경시 css한 줄만 수정하여 여러 페이지의 디자인을 변경 할 수 있게 된다,
- 하나의 Component에 HTML, CSS, JS를 모두 합쳐서 만들 수 있다.
- 독립적이고 재사용가능한 코드로 관리 할 수 있다.
- Component는 function과 비슷. Component도 input을 받아서 return 할 수 있다.

  - React Component는 input을 props라고 한다.
  - return은 화면에 보여져야 할 React 요소가 return된다.

- Component의 종류

1.  function형 Component

```react
function Welcome(props) {
		return <h1>Hello, {props.name}<h1>
   }
```

2.  class형 Component

- React.Component를 extends해서 생성한다.
- Component를 만들 때 필요한 method가 많이 있지만 `render()` method는 필수!

```react
class Welcome extends React.Component {
	render() {
  	return <h1>Hello, {this.props.name}</h1>
      }
     }
```

## Component의 사용

- Component는 function / class 의 이름으로 사용가능하다.

`<Welcom />`

- 원하는 attribute를 얼마든지 추가할 수 있다.
- Component에서 parameter로 해당 attribute를 받아서 사용할 수 있다.
- `props` 라고 한다. (properties 의 줄임말)
- `.(dot)`으로 접근하고 `props.속성명` 으로 속성값을 가져올 수 있다.
- attribute의 이름은 필요에 따라 지어 쓸수있다.

## 더 작은 Component 만들기

- 재사용 가능성이 있다면 Component 로 만든다.
- Component 안에 더 작은 Component를 사용하여 더욱 간결하게 작성 할 수 있다.
