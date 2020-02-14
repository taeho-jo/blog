---
title: 'Git 명령어 & Instargram Clone'
date: '2019-12-04'
category: 'Git'
---

# 1. Git

### 1. git clone

- 말 그대로 원격 저장소를 복제 한다는 뜻
- `**$ git clone <repository> <directory>`** : git의 소스코드를 **지역저장소로 가져오는 것\*\*
- **<repository>** : 원격 저장소의 URL
- **<directory>** : 복제대상의 폴더명
- 따라서 특정 repository를 각자 자신의 local machine에 복사하여 **새로운 저장소를 만드는 것**이다.
- **`git clone -b`** : 특정 브랜치를 clone

### 2. git remote -v

- 이건 조금 더 찾아보고 블로깅

### 3. git status

- git 관리하에 있는 폴더의 작업트리와 index의 상태를 확인
- **`$ git status`** 를 통해서 처음 한 번만 index에 등록하면 추적 대상으로 등록?

### 4. git add

- 파일을 index에 등록하는 명령어
- `$ git add <file>` : 등록할 file 이름
- `$ git add .` : 모든 파일을 index에 등록
- git add까지가 커밋을 위한 준비 단계

### 5. git commit -m "커밋한 내용을 알수 있는 제목"

- git commit -m " " : " " 에는직접 작성을 하고, 변경사항 또는 무엇을 작업했는지 명확히 알 수 있도록 한다.

### 6. git log

- 저장소의 변경 이력을 확인 하는 명령어

### 7. git push origin develop

- 웹 상의 원격 저장소로 변경된 파일을 업로드하는 것을 Git에서는 push라고 한다.
- 원격 저장소에 내 변경 이력이 업로드, 원격 저장소와 로컬 저장소가 동일한 상태가 된다.
- \$ git push origin <branckname>

- branch는 다음에 정리

> http://backlog.com/git-tutorial/kr/

# 2. addEventListener

### **IIFE**(Immediately-invoked function expression : 즉시 실행 함수)

- 함수가 정의되자마자 즉시 실행되는 함수

```javascript
(function() {
  statements;
})();
// 첫번째는 괄호로 둘러싸인 익명함수
// 전역 스코프에 불필요한 변수를 추가해서 오염시키는 것을 방지
// IIFE 내부안으로 다른 변수들이 접근하는 것을 막을 수 있다.

// 두번째 부분은 실행함수를 생성하는 ().
// 이를 통해 JavaScript 엔진은 함수를 즉시 해석해서 실행한다.

(function() {
  var aName = 'Barry';
})();
// IIFE 내부에서 정의된 변수는 외부 범위에서 접근이 불가능하다.
aName; // throws "Uncaught ReferenceError: aName is not defined"
//IIFE를 변수에 할당하면 IIFE 자체는 저장되지 않고 함수가 실행된 결과만 저장
var result = (function() {
  var name = 'Barry';
  return name;
})();
// 즉시 결과를 생성한다.
result; // "Barry"
```

### removeChild

### html tag 중 form/input/button 에 관한 click/enter event내용

(button tag와 a태그는 절대 같이 쓰지 않는다 / form태그에 이벤트 적용시

### 화면이 올라가는 현상 해결법 / button type:submit 이니깐 이벤트 사용)

### querySelector 선언 시에 sysntex 작성 법(\$사용/나눠쓰기)
