---
title: 'IP주소를 이용하여 국가코드로'
date: '2020-01-30'
category: 'next.js'
---

# ip주소를 API를 이용하여 국가코드로 전환

### ip주소를 이용하여 사용자의 국가코드를 알고, 그것을 이용하여 localizaiton까지 구현하기 위함

### 예제로 작성하였습니다.(실제 project는 아님)

- fetch대신 axios를 이용하였다.
- response.data를 data에 저장을한다.
- 그리고 state로 정의해 두었던 countryName, countryCode를 setState하여 저장한다.
- 만약 response.data가 ture라면 localStorage에 저장을한다.

##### 조금 더 다듬어야 할 부분은 mobx로 적용을 시켜야 하고, 또 사용자의 localStorage를 먼저 확인하여 countryCode가 있다면, 바로 진행을하고 없다면 api를 호출할 수 있도록 변화를 줄 것이다.

##### 사용자 접속 -> localStorage에 countryCode가 있는지 없는지 확인

##### -> 있다면 해당 국가에 맞는 언어로 전화되어 랜더링

##### -> 없다면 api를 호출하여 ip주소확인 후 국가코드로 알아내어, localStorage에 저장

```jsx
import React, { Component } from 'react';
import axios from 'axios';

class Example extends Component {
  getGeoInfo = () => {
    axios
      .get('https://ipapi.co/json/')
      .then(response => {
        let data = response.data;
        this.setState({
          countryName: data.country_name,
          countryCode: data.country_calling_code,
        });
        if (response.data) {
          localStorage.setItem('countryCode', data.country_calling_code);
        }
      })
      .catch(error => {
        console.log('dddd', error);
      });
  };

  componentDidMount() {
    this.getGeoInfo();
  }

  constructor(props) {
    super(props);

    this.state = {
      countryName: '',
      countryCode: '',
    };
  }

  render() {
    return (
      <div>
        <p>Country Name: {this.state.countryName}</p>
        <p>Country Code: {this.state.countryCode}</p>
      </div>
    );
  }
}

export default Example;
```
