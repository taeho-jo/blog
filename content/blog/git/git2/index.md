---
title: 'Git을 사용하는 이유'
date: '2019-12-11'
category: 'Git'
---

# Git

- Git이란 소스(code)의 version관리이다.
- 자동으로, 또 체계적으로 관리를 할 수 있게 한다.(코드의 내역관리)
- version을 관리 하는 이유는 과거의 수정내용이나 추가된 내용을 알기 쉽게 하려는 이유와, 문제가 발생했을 시에 돌아갈 수 있게 하고, 유지 보수에 좋기 때문이다.
- version 관리 시스템의 종류는 많이 있지만, git을 대부분 사용한다.

### repository(저장소)

### history(수정 내용의 역사)

- 저장 시점을 commit 이라 한다.
- history는 commit들로 이루어져 있다.

### branch

- branch는 여러개가 있을 수 있고 dafault branch는 master 이다.
- 각각의 branch는 각각의 history가 있다.

```
master   develop    develop2
  |            |           |
  -commit
  |            |           |
  |            |           -commit
  |            -commit
  |            |           |
  |            |           |
  |            |           |
```

### Git이 각광 받는 이유

- 모든 컴퓨터에 모든게 clone이 된다.
- 수정 사항들은 컴퓨터에만 남아있고 공유준비가 끝나면 공유를 한다.
- git사용에는 internet이 필요없다
- 모든 작업은 local에서만 이루어진다.

### github : 공유를 위한 중앙 server

### git Basics

1. Modified (수정된 file) - git status
2. staged (중간 save) - git add (혹시 모를 실수에 대비, 되돌아 갈 수 있게)
3. Committed - git commt.(역사에 남는 순간)

### git 명령어

1. git status
2. git add
3. git commit
4. git diff(수정사항을 보여준다)
5. git log (history를 보여줌)
6. git rm (remove, git 상에서 삭제)
7. git mv (move, 파일을 옮기거나 이름을 바꿀때)
8. git branch
9. git checkout (다른 branch로 옮겨갈때)

- `.`(dot)은 git ignore file, git으로 관리하지 않을 file을 설정

- 함께 작업을 하기 위해서, 또 서로의 작업에 영향을 받지않고 서로 영향을 주지 않기 위해서 각자 branch를 생성한다.
- branch의 이름은 해당 기능의 이름으로 한다. (feature/login)
- 각자 branch에서 작업을 하고 master branch로 합치는 것을 mersing, merse라고 한다.
- master branch로 합칠 때 충돌이 생길 수 있다. 같은 수정사항이 생겼을 때이다. 이것을 conflict라고 한다.
- Github에서 local로 git clone 을 진행하고 각자의 branch를 만들어 작업을 한 후 git push를 한다. git push를 진행할 때 git pull을 먼저 선행하게 된다. 다른 사람이 먼저 수정한 사항을 반영하기 위해서이다.
