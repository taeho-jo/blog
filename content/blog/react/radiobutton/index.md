---
title: '[React] Radio Button - 라디오버튼'
date: '2020-09-27'
category: 'react'
---

기능들을 구현하면서,
간단한 것들이라도 기록하기 위해서 블로그를 작성해야겠다고 생각이 들어서

아주 간단한 라디오버튼으로 시작을 해보려고 합니다.

![](https://images.velog.io/images/jotang/post/c3ded584-ac6b-4f91-a73b-2486f4fd5e29/image.png)

라디오 버튼은 html의 input 태그의 `type='radio'`를 통해서 간단하게 불러올 수 있습니다.

```jsx
<RadioBtn type='radio' id='radio' />
<label htmlFor='radio'>Radio</label>
```

RadioBtn은 styled-components를 사용해서 만들었습니다.(실제로는 Input태그)

type을 radio로 지정해주고, 
`<label>Radio</label>`
label 태그를 이용하여 버튼의 이름도 만들어줍니다.

input 태그의 id와 label 태그의 htmlFor속성을 이용하여 라디오버튼과 연결을 해줍니다.

연결을 하게되면, 버튼말고 Radio라는 글을 클릭해도 라디오버튼이 동작하게 됩니다.


```jsx
const [ inputStatus, setInputStatus ] = useState(false)

const handleClickRadioButton = () => {
  setInputStatus(!inputStatus)
}

<RadioBtn 
  type='radio' 
  id='radio' 
  checked={inputStatus} 
  onClick={handleClickRadioButton} />
<label htmlFor='radio'>Radio</label>
```

라디오버튼은 checked의 `ture/false` 에 따라 동작을 합니다.

`handleClickRadioButton` 함수를 통해서 라디오버튼을 동작할 수 있게 되었지만,

true / false로 라디오버튼의 상태를 관리하게 된다면,
라디오버튼의 갯수만큼 상태도 관리를 해야겠죠..

저는 그래서 함수의 인자로 input의 이름을 전달해 주는 방법을 생각했습니다.


```jsx
const [ inputStatus, setInputStatus ] = useState('')

const handleClickRadioButton = (radioBtnName) => {
  setInputStatus(radioBtnName)
}

<RadioBtn 
  type='radio' 
  id='radio' 
  checked={inputStatus === 'radio'} 
  onClick={() => handleClickRadioButton('radio')} />
<label htmlFor='radio'>Radio</label>
```

state의 상태와 정해준 이름이 같다면, true가 되어서 불이 들어 올 것입니다.

![](https://images.velog.io/images/jotang/post/78bd6633-a36d-475e-97aa-fd6c74853bf6/image.png)

라디오버튼은 체크박스와는 달리 한가지만 선택되어질 때, 많이 사용하는 것으로 알고있습니다.

제가 구현하는 방식이 좋은 방법인지, 또 제가 작성한 코드가 좋은 코드인지 사실 아직 확신은 없습니다.

더 좋은 방법과 더 효율적인 코드가 있다면, 알려주시면 너무나 감사드려요!

제가 작성한 코드는
[Github](https://github.com/taeho-jo/various_functions)에서 확인할 수 있습니다!!

아래 링크에서는 라디오버튼을 직접 확인할 수 있습니다!!

[Various_Functions](https://variousfunctions.netlify.app/)

볼품없는 글 읽어주셔서 감사드려요~