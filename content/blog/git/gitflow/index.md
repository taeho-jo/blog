---
title: 'Git Flow'
date: '2020-03-17'
category: 'Git'
---

# Git Flow

이전에 살펴보았지만, git은 **version관리 시스템**이다.
git을 사용하는 이유는 다른 version 관리 시스템들에 비해서 협업에 더욱 좋다.

우리들은 git을 사용하게 될 때, 기능별로 branch를 나누게 된다. defalut branch는 당연히 master이다.

어떻게 git을 사용하고, 어떻게 branch를 나누어야 더 효율적인가에 대해 고민이 필요한 시점이다.

결국 **Git Flow**란 git을 `어떻게 더 효율적으로 사용해야하는가?` 에 대한 특정한 `패턴 또는 프로세스` 라고 할 수 있다.

**git flow**에는 5가지의 branch가 있고, 아래에서 알아보도록 하겠습니다.

![](./gitflow.png)

# 1. master

> 배포되었거나 배포될 소스가 저장되는 branch

master branch는 배포가 될 때마다 태그만 달아주는 형식으로 관리를 한다.
예를들어서 제품1.0.0 , 제품 1.2.1 등의 배포버전이 있으면 해당 버전에 태그를 달아서 언제든 원하는 버전의 소스를 받아볼 수 있게 하는 역할하게 된다.

# 2. develop

> develop branch는 여러 명의 개발자가 함께 공유하면서 개발을 진행하는 브랜치다.

하지만 이 develop brnach에 직접적으로 다 같이 개발을 하는 것은 아니다. develop branch는 개발자들이 각자 자신의 로컬에서 기능별로 branch를 생성하여 개발을 진행하고, 개발 완료 시에 develop branch에 push하거나, Pull Request를 보낸 후 코드 리뷰 등을 진행한 후에 develop branch에 merge하게 된다.

`결국 실제 개발이 진행되는 branch는 develop branch이다.`

# 3. feature

> 각 개발자에 의해 기능 단위 개발이 진행되는 branch

새로운 기능을 추가하기 위해 사용되는 branch이며 origin에는 업로드하지 않고, 개발자의 local에서만 존재.
feature branch는 특정 기능 개발이 필요할 때 develop branch로부터 파생되어 나오게 되고, 기능 개발이 완료되면 다시 develop branch로 병합된다.

feature branch의 이륾은 `master, develop, release, hotfix 등 제외`
(feature/기능이름)

# 4. release

> 내부적으로 배포할 준비가 되었다고 생각되는 소스가 저장되는 브랜치

개발한 기능들을 develop branch에서 따와서, release branch를 만든다. 배포를 하기 전, release branch에서 버그들을 체크하게 되고 발견되는 버그들을 수정하게 된다. 버그들을 다 수정을 진행했다면, 배포를 위해 master branch, 그리고 수정된 버그들을 적용하기 위해서 develop branch에 병합한다. 병합 후에 release branch는 삭제한다.

release branch의 이름은 `release/*, release-*`로 한다.

# 5. hotfix

> 배포 버전에 생긴 문제로 긴급한 트러블슈팅이 필요할 때 개발이 진행되는 브랜치

master branch에 배포되어 있는 product에서 발생하는 버그들을 수정하기 위한 branch. hotfix branch는 master로 부터 파생되어 나오고, 수정한 버그들을 적용하기 위해서 master와 develop branch에도 병합 시킨다.
만약 release 브랜치가 존재한다면 release 브랜치에도 hotfix 브랜치를 병합하여 다음 릴리즈때 내용이 적용되도록 해야 한다.

hotfix branch의 이름은 `hotfix/*, hotfix-*`로 한다.
