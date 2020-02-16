---
title: 'React Native CounterApp만들기'
date: '2020-02-16'
category: 'react native'
---

# Counter App 만들기

##### 세팅은 앞의 블로깅한 내용과 똑같이 하였다.

##### Hooks & TypeScript & styled Components를 사용할 것이다.

- 최종 모습

  ![](https://images.velog.io/images/jotang/post/158f5068-a18e-4eda-b8c5-05a50376e2de/image.png)

## 1. Button Components 만들기

#### Props의 타입 정해주기

- 사실 TypeScript는 이전에 사용해 본 적이 없고, 이번 native를 하면서 지금 이 순간에 처음 접하게 되었다.

- components의 props 타입을 지정함으로써, type에 대한 버그와 에러를 줄일 수 있다.

#### 전체코드

```jsx
import React from 'react';
import styled from 'styled-components/native';

interface Props {
  iconName: 'plus' | 'minus';
  onPress?: () => void;
}

const Button = ({ iconName, onPress }: Props) => {
  return (
    <Container onPress={onPress}>
      <Icon
        source={
          iconName === 'plus'
            ? require('~/assets/images/plus.png')
            : require('~/assets/images/minus.png')
        }
      />
    </Container>
  );
};

export default Button;

const Container = styled.TouchableOpacity``;
const Icon = styled.Image``;
```

<br />
<br />

#### 부분코드

- TypeScript의 `interface Props{}` 를 사용하여 Props의 타입을 정해준다.

```tsx
interface Props {
  iconName: 'plus' | 'minus';
  // iconName은 plus와 minus라는 string만 받는다. " : "는 필수항목을 나타낸다.
  onPress?: () => void;
  // onPress는 반환값이 없는 함수로 설정. " ?: " 는 필수항목은 아님을 나타낸다.
  // iconName은 설정하지 않으면 error가 발생하고,
  // onPress는 설정하지 않더라도 error가 나지 않는다.
}

const Button = ({iconName, onPress}: Props) => {
```

<br/>

- Container는 react-native의 `TouchableOpacity` 컴포넌트를 이용하였다.

- **TouchableOpacity** : View가 터치에 적절하게 응답하도록하는 컴포넌트이다. 터치를 하여 아래로 눌러지게 되면 래핑된 뷰의 불트명도가 감소하여 흐리게 표시된다. - **activeOpacity** : 기본 속성으로 `activeOpacity`가 있다. default값은 0.2이고, 0~1까지 값을 지정해 줄 수 있다.

```jsx
<Container activeOpacity={0.5} onPress={onPress}>
  ...
</Container>
```

<br/>

```jsx
<Icon
  source={
    iconName === 'plus'
      ? require('~/assets/images/plus.png')
      : require('~/assets/images/minus.png')
  }
```

- Icon은 `Image` 컴포넌트를 이용하였다.

- html과는 다르게 `source` 에 경로를 지정하게 되고 `require` 구문을 사용하게 된다.

- imgae는 `src/assets/images` 폴더안에 들어가게된다.

- 3가지 사이즈의 이미지라면, `add.png / add@2x.png / add@3x.png` 로 이름을 변경하여 넣어준다. react-native는 해당 단말기의 화면 사이즈에 맞는 image를 불러와 랜더링 하게 된다.

## 2. Counter Components 만들기

#### 전체코드

```jsx
import React, { useState } from 'react';
import styled from 'styled-components/native';
import Button from '~/components/Button';

interface Props {
  title?: string;
  initValue: number;
}

const Counter = ({ title, initValue }: Props) => {
  const [count, setCount] = useState < number > 0;

  return (
    <Container>
      {title && (
        <TitleContainer>
          <TitleLabel>{title}</TitleLabel>
        </TitleContainer>
      )}
      <CounterContainer>
        <CounterLabel>{initValue + count}</CounterLabel>
      </CounterContainer>
      <ButtonContainer>
        <Button iconName="plus" onPress={() => setCount(count + 1)} />
        // 필수항목인 iconName은 꼭 설정하여준다.
        <Button iconName="minus" onPress={() => setCount(count - 1)} />
      </ButtonContainer>
    </Container>
  );
};

export default Counter;

const Container = styled.View`
  flex: 1;
`;

const TitleContainer = styled.View`
  flex: 1;
  justify-content: center;
  align-items: center;
  /* padding-top: 30px; */
`;

const TitleLabel = styled.Text`
  font-size: 44px;
  color: #fff;
  font-weight: bold;
`;

const CounterContainer = styled.View`
  flex: 2;
  justify-content: center;
  align-items: center;
`;

const CounterLabel = styled.Text`
  font-size: 100px;
  font-weight: bold;
  color: #fff;
`;

const ButtonContainer = styled.View`
  flex: 1;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: space-around;
`;
```

#### 부분코드

```jsx
interface Props {
  title?: string;
  // 필수항목이 아닌 title, type은 string
  initValue: number;
  // 필수항목인 initValue, type은 number
}

const Counter = ({title, initValue}: Props) => {
```

<br/>

- `const[ 변수명, 변수를 변경할 set함수 ] = useState<state의 type>(초기값);`

```jsx
const Counter = ({title, initValue}: Props) => {
  const [count, setCount] = useState<number>(0);
  render(
    ...
  )
}
```

<br/>

- object의 형식도 가능하다.

```jsx
interface Props{...}
interface State{
  name: string;
  age: number;
}
...
const [ user, setUser ] = useState<state>({
  name: "홍길동",
  age: 20
})
```

## 3. App.tsx

#### 전체코드

```jsx
import React from 'react';
import styled from 'styled-components/native';
import Counter from './screens/counter';

interface Props {}

const App = ({  }: Props) => {
  // props가 없지만 type은 설정해 준다.
  return (
    <Body>
      <Counter title={'This is Counter App'} initValue={1004} />
      // 필수 항목인 initValue를 작성해 준다.
    </Body>
  );
};

export default App;

const Body = styled.View`
  background-color: #000;
  opacity: 0.85;
  flex: 1;
`;
```

#### 그 외

- class형 components로 만드는 예제가 있었지만, hooks가 더 효율적이라고 생각 해서 따로 블로그 정리는 하지않습니다. 다만, 따로 챙겨보았다는 사실을 잊지말아주세요

- styled components를 작성할 때, flex부분이 일반 react와는 차이가 있습니다. 일반 react에서는 `display: flex;` 라고 값을 주지만, native에서는 `flex: 1;` 이라고 작성

- `flex: 1;` 의 의미가 화면 전체를 사용하겠다는 의미로 받아드렸습니다. flex: 1이 선언된 컴포넌트의 자식이 둘이라면 `flex: 1;` 을 선언한 컴포넌트와 `flex: 2;` 를 선언한 컴포넌트가 있다면 `1:2의 비율`로 화면을 나눠가지게 됩니다.

- react-native에서는 react와는 다르게 flex의 default가 column입니다. 아래로 싸이게 됩니다. 가로로 놓이게 하고 싶다면 `flex-direction: row;`를 작성해야합니다.
