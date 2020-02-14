---
title: 'Enemyrain ( Image Sprite, parseInt )'
date: '2019-12-05'
category: 'JavaScript'
---

# 1. Problems & Solution

### 1. problems

1. hero의 left value 의 type이 string. number로 변화하여 계산.

2. image sprite

### 2. solution

1. parsInt를 사용하여 string에서 number로 변화하여 사용하였다

```javascript
let heroLeftValue = parseInt(heroStyle.left);
console.log(heroLeftValue); // px를 제외한 value를 반환
$hero.style.left = heroLeftValue - 10 + 'px';
// number 끼리 계산을 해주고 'px'를 더해서 이동
```

2. background-position 속성을 이용하였다. 아래에서 자세히 알아보겠다.

# 2. Image Sprites(background-position)

- Image Sprites는 하나의 이미지 안에 들어 있는 이미지들의 모음이다. 많은 이미지를 로드하려면 오랜 시간이 걸리고, 서버에 여러번 요청을 해야한다. Image Sprites를 사용하면 서버의 요청 수를 감소시키게 된다.

![hero.png](https://images.velog.io/post-images/jotang/9af51150-190e-11ea-9838-d539f766794d/hero.png)

- hero 한명의 width를 계산하여 div를 생성하고 CSS에서 background-image를 이요하여 배경화면으로 image를 삽입한다.
- background-position 속성을 이용하여 image의 위치를 변경한다.

![KakaoTalk_20191208_013747248.jpg](https://images.velog.io/post-images/jotang/050c8860-1910-11ea-9838-d539f766794d/KakaoTalk20191208013747248.jpg)

```javascript
.hero {
  background-positon : 0 0;
}
//CSS 사용법. background-positionX, background-positionY 를 사용하여 각각 값을 입력할 수 있다.
$hero.style.backgroundPosition = '-70px 0'
//JavaScript 사용법. 역시 backgroundPositionX,backgroundPositionY 사용가능 하다.
```

# 3. parseInt

```javascript
let heroLeftValue = parseInt(heroStyle.left);
//hero의 left value를 number로 반환한다.
let ghostTop = parseInt(ghostTopValue.top);
//ghost의 top value를 number로 반환한다.
```

- parseInt() 함수는 string의 구문을 분석하여 특정 진수의 정수를 반환한다. 여기서 특정 진수는 수의 진법 체계에 기준이 되는 값을 말한다.
  `parseInt(string, radix);`
- string: 분석할 value. string이 아니면 string으로 변환한다. string의 앞 공백은 무시한다.
- radix: string이 표현하는 정수를 나타내는 2와 36사이의 진수/
  > https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/parseInt
