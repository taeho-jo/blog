---
title: 'Responsive Web을 위한 media query'
date: '2020-01-02'
category: 'css'
---

# 반응형 웹(Responsive Web)

- 레이아웃이 브라우저의 크기에 따라 (화면의 크기를 줄였다 늘렸다 할때) 아주 자연스럽게 바뀌는게 하는 것.

* 간단히 말로 예를 들어보자면 크기가 일정 이상이면 보이고 일정 이하면 안보이고를 설정 할 수 있다. (css => display : none)

### media query

- 반응형 웹을 구현하는 CSS Technique이다.

### @media (CSS)

```scss
@media only screen and (max-width: 480px) {
  body {
    font-size: 12px;
  }
}
```

1. **@media** — 이 키워드는 media query를 시작하겠다는 의미입니다.
2. **only screen** — 어떤 디바이스에서 적용하는지 알려줍니다. 예를 들면 프린트를 하고싶을 때 적용하려면 only print라고 작성하면 됩니다. screen이라고 할 경우 어떤 디바이스에 상관없이, 화면에 보이는 스크린이기만 하면 전부 적용됩니다.
3. **and (max-width : 480px)** — 이건 media feature라고 불리는 부분입니다. 어느 조건에 아래의 css를 적용할지 작성해줘야 합니다.

```scss
@media only screen and (min-width: 320px) and (max-width: 480px) {
/* ruleset for 320px - 480px */
}
(media query를 320px과 480px 사이의 화면 크기에서 적용하겠다의 의미)
```

- media query를 사용할 때 screen이 ~보다 작을 때의 경우는 max-width 를 , ~보다 클때의 경우는 min-width 를 사용하여 조건을 정해주면 된다.
- 보통은 media query를 사용한다는 명령어( `@media` )를 사용한 후에 해당되는 selector에 적용 하여 style을 적어주면 된다.
- 미디어 쿼리에서는 px단위는 사용하지 않고, %단위를 사용하게 된다.

### 미디어쿼리를 할 때 기준점(break point)

- 반응형 웹을 만들 때 어디크기에서 부터 media query를 시작하여 변화를 줄 것인가에 대한 기준점을 **break point**라고 한다..

- **break point**가 많으면 편한데 관리하기가 너무 힘들다. css가 너무 늘어남

- 보통 회사에서는 2~3개의 break point를 가지게 된다.

- 함께 작업을 하게 되는 경우에는 scss에서 **break point**를 변수로 지정해서 import를 통해서 가져다 쓰는 것이 좋다. 변수만 지정하는 scss를 파일을 만드는 것도 하나의 방법

- css는 아래에 있는 것이 우선순위이다. media를 쓸 때 항상 media를 밑에 써야한다. 우선순위를 잘 생각해야 한다.
