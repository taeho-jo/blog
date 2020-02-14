---
title: 'justSell Project'
date: '2020-01-13'
category: 'react'
---

# Problems & Solution

## problems

![image.png](https://images.velog.io/post-images/jotang/8c74fd90-3612-11ea-a126-a39412bb9e36/image.png)

- 화살표 click시에 해당 월이 증가하여야하고, 다음달과 전달 역시 증가하여한다.
- 12월이 되었을 때 그 다음 달은 1월이 되어야한다.
- 해당 월이 12번 증가했을 때 년도 역시 1씩 증가해야한다.

## soultion

- 각각 state로 관리를 한다.

![image.png](https://images.velog.io/post-images/jotang/df849220-3612-11ea-a126-a39412bb9e36/image.png)

- 달과 년이 증가하는 방법은 button이 한 번 click 될 때 마다 1씩 증가 시키고,
  만약 해당 숫자가 12가 된다면 달의 state를 1로 설정하고 year의 state에 1을 더한다.

```jsx
const plusChange = () => {
  setMonth(month + 1);
  setNextMonth(nextMonth + 1);
  setLastMonth(lastMonth + 1);
  if (month === 12) {
    setMonth(1);
    setYear(years + 1);
  }
  if (nextMonth === 12) {
    setNextMonth(1);
  }
  if (lastMonth === 12) {
    setLastMonth(1);
  }
};
```

- 거꾸로 감소하는 방법은 증가하는 방법의 반대로 click이 될 때 1씩 감소 시키고,
  만약 해당 숫자가 1이 된다면 달의 state를 12로 설정하고, year의 state에 1을 뺀다.

# styled-components

```jsx
const Info = styled.div`
  width: 230px;
  height: 50px;
  background: #eaf8ff;
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 10px 0 5px 0;
  border-radius: 10px;
`;

const Info2 = styled(Info)`
  background: #e5f9f3;
`;
//background 색상만 다른 똑같은 모양의 components
```

- 같은 tag를 사용하고, 크기와 속성들이 똑같고, 단지 하나의 색상정도만 다르다면 똑같이 작성하지말고
  styled(복제할styled-components) 를 적고 사용한다면 더욱 간편하게 styled-components를 만들 수있다.
