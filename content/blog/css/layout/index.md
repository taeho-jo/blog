---
title: 'css 레이아웃'
date: '2019-11-25'
category: 'css'
---

# 1.position 속성 - relative, absolute, fixed

### **1.0 position**

- 레이아웃을 배치하거나, 객체의 위치를 설정

### **1.1 static**

- position 속성의 default 값. 잘 사용하지 않음

### **1.2 relative**

- 기본적으로 표시된 위치를 기준으로 새로운 위치를 지정할 수 있다.
- **top, bottom, left, right**를 이용하여 새로운 위치를 지정하지 않는 이상 기본적인 위치와 똑같다.
- 새로운 위치로 이동하여도 **기존의 자리는 그대로 남아있다.**

### **1.3 absolute**

- 해당되는 요소의 **부모 요소를 기준**으로 위치가 지정된다.
- 부모요소에 static 또는 relative속성이 지정되어 있어야 한다.
- **대상 자체가 이동한다.**

### **1.4 fixed**

- **viewport에 고정**된다.
- 스크롤을 해도 지정된 곳에 항상 위치한다.

```css
div.nav {
  position: fixed;
  top: 0;
}
```

이라면 viewport 상단에 항상 고정된다.

# 2. display ( block, inline, inline-block )

### **2.1 block**

- 아래로 배치된다. **width, height** 값을 지정할 수 있다.
- **margin, padding** 의 값은 상하좌우 모두 반영된다.

ex) `div`, `p`

### **2.2 inline**

- block요소와는 다르게 옆으로 배치
- 단락 안에서 해당 단락의 흐름을 방해하지 않은 채로 텍스트를 감쌀 수 있다.
- **width, height**의 값을 줄 수 없다.
- **margin, padding** 의 값은 좌우 간격만 반영되고, 상하 간격은 반영되지 않는다.

ex) `span`, `a`

### **2.3 inline-block**

- block요소와 inline요소의 특징을 함께 가지게 된다.
- inline-block으로 지정되면 기본적으로 inline 요소처럼 줄바꿈 없이 **한 줄에 배치**된다.
- inline요소와는 다르게 **width, height 값을 지정** 할 수 있다.
- **margin, padding의 좌우 0간격 뿐만아니라 상하 간격 역시 지정** 할 수 있게 된다.

# 3. float

### **3.1 float**

- 기본적으로 float는 이미지와 텍스트를 함께 배치하기 위해서 만들어진 속성
- 레이아웃을 구성할 때 역시 자주 이용되고 있다.
- 요소를 좌우 방향으로 정렬 할 수 있다.
- 기본값은 **none**이고 **left**와 **right**를 사용할 수 있다.

우측에 띄움 `.box { float: right; }`
좌측에 띄움 `.box { float: left; }`

### **3.2 overflow**

- float 속성을 사용했을 때 이미지가 텍스의 영역을 침범해서 배치가 될 경우 float를 가진 요소의 부모 요소에 overflow속성을 사용
- overflow 가 사용 할 수 있는 값은 **hidden, auto** 등이 있다.

### **3.3 clear**

- float의 문제를 해결하기 위해 사용되는 속성
- clear 속성이 지정된 영역 이후로는 float은 작동되지 않는다.
- claer가 사용할 수 있는 값은 **left, right, both**

### **3.4 가상요소선택자**

- 가상요소선택자 **`::after`** 를 사용할 수 있다.
- `clearfix`라는 클래스를 해당 부모 요소에 부여

```
clearfix::after {
	content: '';
    display: block;
    clear: both;
}
```

- 위 코드 처럼 사용 가능 하다.

###### 위 1일차 블로그는 네이버블록그에 미리 작성했던 것을 옮긴 것 입니다.
