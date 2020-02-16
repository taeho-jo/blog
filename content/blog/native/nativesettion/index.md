---
title: 'React Native 초기세팅!'
date: '2020-02-14'
category: 'react native'
---

# React Native

##### 지난 번의 세팅은 조금 이상한 것 같아 다시 학습 중입니다.

##### expo를 사용하지 않고 설정을 진행할 예정입니다.

##### 아시는 것과 다를지도 모릅니다.

##### 본인은 window환경이기에 window환경설정만 진행합니다

# Chocolatey, Node, Python, Java개발 킷 설치

#### cmd를 관리자 권한으로 실행시켜 진행합니다.

- **chocolatey** : chocolatey 홈페이지에 접속하여 다운받습니다.
  [chocolatey 다운로드 페이지](https://chocolatey.org/)

- **node** : `choco install -y nodejs.install`, 버전확인은 `node --version`

- **python** : `choco install -y python`, 버전확인은 `python --version`

- **java개발 킷** :`choco install -y jdk8`, 버전확인은 `java --version`

# react-native-cli & android studio

- **react-native-cli** : `npm install -g react-native-cli`

#### android studio 설치

[Android Studio 다운로드 페이지](https://developer.android.com/studio/index.html)

- 설치과정에서는 특별히 건드릴 것이 없기에 그냥 진행하면 된다.
- 설치 완료 후에 실행을 시키고나면, 오른쪽 하단에 `configure`을 선택하고 `SDK Manager`를 선택한다.
- `Show Package Detaiils`를 선택하고 개발할 SDK버전을 선택하고 다운로드 한다.
- android studio의 환경변수를 설정한다. `내PC`를 우클릭하여 속성으로 들어간다.
- `고급 시스템 설정` 에서 환경변수를 선택하여 이동한다. 새로 만들기를 하여 사용자 변수에 추가한다.
  ![](https://images.velog.io/images/jotang/post/83dd8076-3e4c-434e-86cd-d299811e26f4/image.png)
- 변수이름은 `ANDROID_HOME`으로 하고, 변수 값은 안드로이드 SDK의 위치를 입력하는데, 이 위치의 값은 android studio에서 확인 할 수 있다.
  `(Configure > SDK Manager에서 상단의 Android SDK Location에서 확인 가능)`
- 마지막으로 android studio의 platform tools의 위치를 설정한다.
  ![](https://images.velog.io/images/jotang/post/c77200f0-da1a-4091-99c8-75e8e0439b24/image.png)
- Path에서 편집을 눌러 이동한다. 새로만들기를 클릭하여
  `C:\Users\[사용자이름]\AppData\Local\Android\Sdk\platform-tools` 를 작성

# react native project생성!

#### 보통은 VScode의 터미널을 이용했지만, 이번에는 git base 터미널을 이용하여 설치를 진행하였다.

#### node package manager를 통하여 설치하는 라이브러리, 모듈들이 버전을 고정하기 위하여 `npm config set save-exact=true` 를 하였다.

- `react-native init JotangFirstNative`
- 특정 버전으로 설치하고 싶다면 `react-native init -version 0.59.10 FirstApp` 으로 생성도 가능하다.

- project가 생성되었다면 실행을 시켜봐야한다~!!!!!
- android에서는 에뮬레이터로 실행하거나 안드로이드 단말기를 usb로 연결한 후 진행한다
  `react-native run-android` 또는 `npm run android (<- 최신버전 기준)`

###### native에 대해서 계속해서 업로드 할 수 있도록 노력하겠습니당
