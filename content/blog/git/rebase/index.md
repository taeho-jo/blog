---
title: 'unit test & Git rebase'
date: '2020-01-06'
category: 'Git'
---

# Unit test

## test

- 개발에서 나의 코드가 실행이 잘되는지 안되는지 확인은 필수이다.
- 작동이 되지 않는 코드는 전혀 필요없다. 코드를 안 짠거보다 좋지 않다.
- 개발자에게 개발과 test의 비율은 50:50
- **test는 자동화가 되어야한다.**
- **실행이 빨라야한다.**

### 1. UI Testing / End-To-End Testing

- 직접 클릭해보고, 직접 검색해보고, 직접적으로 확인해보는 방법
- 처음부터 끝까지 다 해본다고 해서 End-to-End
- 금액적으로 비싸다. 백엔드,DB, 프론트 모든 영역을 연결시켜야하고(거의 준배포 수준), 테스트 시 사람이 직접 테스트해야한다(인건비), 실수가 많을 수 밖에 없고 시간이 오래걸린다. (회사의 입장에서는 비용과 시간이 가장 많이 들지만 가장 직관적이고 하기가 쉽다.)

### 2. Integration Testing

- 해당 서버에 한정해서 test.
- 프론트라면 mockdata를 통해서 rendering이 잘되는지 확인, 백이라면 서버를 열고 postman등 tool을 이용하여 원하는 응답이 오는지 확인
- 비싸지는 않지만 어느정도의 공수가 많이든다.

### 3. Unit Testing

- unit : test할 수 있는 가장 작은 단위
- 코드를 test한다.(function 함수를 테스트 하는 것)
- 함수호출을 통해서 test
- 코드로 코드를 test한다.
- 결국 나의 코드(함수)를 test 할 수 있는 code를 짜는 것.
- 반복적으로 돌려서 사용할 수 있다.
- bug 가 발견되어도 그 코드만 고치면 된다.
- 굉장히 중요
- 아주 간단한 코드라도 사람이기 때문에 실수를 할 수 있고, 둘째는 지금 당장은 너무 간단하고 문제가 없을 수 있으나, 나중에 남이 그 코드를 수정하거나 덧붙일 수 있다.이때 유닛테스트를 안한다면 원래의 코드가 잘못된게 아니라고 말할 수 가 없게 된다.

![image.png](https://images.velog.io/post-images/jotang/6ee27d60-302d-11ea-85a0-e598498faac6/image.png)

## test의 일반 원칙

- 각 test는 독립적이어야 한다. (stateless)
- 독립적이지 않다면 순서에 따라 결과가 달라지게 될 수 도 있고, 넘어가는 경우가 생길 수 있다.
- 다음 test가 있기 전 지워줘야한다.

[unit test 자료 정보](https://stackoverflow.com/c/wecode/questions/157/158#158)

# Git rebase

## merge : master에 branch를 합치는 것

- master branch에 서로 다른 branch들을 합치는 것
- merge는 merge commit이 남게된다.
- 다수가 작업하게 될 때 histort가 복잡해진다.(개발 내역을 보기가 힘들어진다.)

#### fast forward (나의 branch가 최신일 때)

## rebase

- merge와는 달리 branch를 만들었던 시점의 base를 새롭게 설정한다.((feature branch의 시작점을 다시 조정한다)
- rebase는 역사를 바꾸는 것이다.
- histoty line을 깔끔하게 하기 위해서 사용한다. (history를 한 줄로 만든다)
- merge는 commit이 많아도 한번에 되지만, rebase는 순서대로 정렬되어야하고 따라서 conflict도 많이 나게 된다.

## squash

- rebase에는 squash가 동반되어야하는데 squash란 여럿 commit을 하나로 만드는 것이다.
- 여럿 commit을 하나의 commit으로 만들고 단 하나의 commit message를 남긴다.

![rebase.jpeg](https://images.velog.io/post-images/jotang/4dec0f00-353b-11ea-acdf-457f933d7877/rebase.jpeg)
