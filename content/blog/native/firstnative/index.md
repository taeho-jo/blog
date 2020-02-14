---
title: 'React Native 맛보기'
date: '2020-02-09'
category: 'react native'
---

# react native

##### 맛보기이 때문에 정확하지 않은 정보가 다수 포함되어 있을 수 있습니다.

##### 추후에 정확히 공부해서 다시 업로드 예정입니다.

- `npx create-react-native-app 폴더명`
- 위의 명령어로 설치를 한다. 나의 경우에는 expo cli이 없다고 설치하겠냐고 물어서 당연히 yes를 눌렀다.
- 그리고 나면 해결쓰 (사실 중간에 5가지중에 선택하는 것이 있었는데 스샷을 못찍었습니다.)
- 맨위에 있는 것을 선택.

# 타노스 앱

### 오늘의 튜토리얼

### random숫자를 이용하여 0.5이하면 사망, 0.5이상이면 생존

![image.png](https://images.velog.io/post-images/jotang/096d2fa0-4b32-11ea-81f5-4d948c744b9d/image.png)

- `AsyncStorage` 브라우저의 local,sessionStorage와 같은 역할을 한다.

```jsx
import React, { Component } from 'react';
import { StyleSheet, Text, View, AsyncStorage, Button } from 'react-native';

export default class App extends Component {
  click = () => {
    const newNumber = Math.random();
    this.setState({
      thanosNumber: newNumber,
    });
    AsyncStorage.setItem('thanosNumber', newNumber.toString());
  };

  excute = async () => {
    const result = await AsyncStorage.getItem('thanosNumber');
    if (result) {
      this.setState({
        thanosNumber: Number(result),
      });
    } else {
      this.click();
    }
  };

  constructor() {
    super();
    this.state = {
      thanosNumber: null,
    };
    this.excute();
  }

  result = () => {
    const { thanosNumber } = this.state;
    let resultText = '';
    if (thanosNumber === null) {
      resultText = '';
    } else if (thanosNumber < 0.5) {
      resultText = '우주의 균형을 위해 먼지가....';
    } else {
      resultText = '당신은 살아남았습니다!!';
    }
    return <Text style={styles.text}>{resultText}</Text>;
  };

  render() {
    return (
      <View style={styles.container}>
        {this.result()}
        <Button title={'Click~~here~'} onPress={this.click} />
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
  text: {
    color: '#27ae60',
    fontSize: 26,
    fontWeight: 'bold',
    marginBottom: 30,
  },
});
```

### 다음에 조금 더 내용과 개념을 보강해서 돌아오도록 하겠습니다.
