---
title: 'Closure'
date: '2020-03-08'
category: 'JavaScript'
---

한 번에 완전히 이해하기에 쉽지 않은 개념이지만, 오늘은 JavaScript 중 closure에 대하여 학습해보았습니다. 틀린 부분이 있다면 댓글로 따끔히 혼내주세요.

# closure란 무엇인가?

- 함수가 자신이 선언됐을 때의 환경인 스코프를 기억하여 자신이 선언됐을 때의 환경 밖에서 호출되어도 그 환경에 접근할 수 있는 함수를 뜻한다.

```js
const a = () => {
  let name = 'jotang';
};
```

- 함수 a안에서 선언된 name이라는 변수는 function scope의 변수.
- scope 밖에서 name에 접근을 하더라도, jotang이라는 값을 얻을 수 없다.
- scope 밖에서 name이라는 변수를 선언하더라도, scope 안의 name 변수에는 어떠한 영향도 주지 못한다.

```js
const a = () => {
  let name = 'jotang';
};
console.log(name); // error

const a = () => {
  let name = 'jotang';
  console.log(name);
};
a(); //console.log() 가 호출되어 'jotang'
```

- JavaScript에서 함수는 함수 그 자체가 다른 값들과 똑같이 **값으로써** 취급당하는 것이 가능.
- 함수는 인자로 어떠한 값을 받고, 어떠한 값을 리턴
- 함수는 또 다른 함수를 인자로 받거나 리턴하는 것도 가능
- 함수 안에 함수를 선언하는 것 역시 가능

```js
const a = () => {
  let name = 'jotang';

  const consoleName = () => {
    console.log(name);
  };

  consoleName();
};
// a() => consoleName() => console.log() => 'jotang'
```

```js
const a = () => {
  let name = 'jotang';

  const consoleName = () => {
    console.log(name);
  };

  return consoleName;
};
// a() 는 함수인 consoleName 그 자체를 return 한다.
// consoleName() 호출이 되는 것이 아니다

const result = a();
// result는 결국 a()의 return 값인 consoleName 함수이다.
// result는 함수라고 부를 수 있다.

result();
// result() => consoleName() => console.log() => 'jotang'
```

- result 함수는 어떠한 function scope 안쪽 값에 접근 할 수 있는 상태가 되었다.

#### 이렇게 function scope로 보호된 어떤 변수에 대해 외부에서 접근할 수 있게 해주는 함수를 JavaScript에서 `Closure` 라고 부른다.

# closure

- closure를 생성하는 함수가 입력을 받아서 그 입력값을 closure가 사용하는 경우가 많다.

```js
const makeClosure = value => {
 const innerValue = value;

 const getValue = () => {
  retrun innerValue;
 }

 return getValue
}

const getInnerValue = makeClosure('하이루')
getInnerValue() // '안녕하세요'
innerValue // Error
```

- 여기서 makeClosure에 의해 리턴된 getValue 함수가 담긴 변수 getInnerValue를 우리는 closure라 부를 수 있다.

- function scope 밖에서 innerValue에 접근하기 위해서는 closure인 getInnerValue를 통해서만 접근 할 수 있게 된다.

```js
const makeClosure = initialValue => {
  let innerValue = initialValue;

  const getValue = () => {
    return innerValue;
  };

  const setValue = nextValue => {
    innerValue = nextValue;
  };

  return {
    get: getValue,
    set: setValue,
  };
};

const innerData = makeClosure('안녕하세요');
innerData.get(); // '안녕하세요'
innerData.set('잘가세요');
innerData.get(); // '잘가세요'
```

- closure 함수 내부의 값을 읽는 것 뿐만이 아니라, 값을 수정하는 것 역시 가능

### closure를 활용한 교통카드

```js
const createCard = string => {
  let status = false;
  let pw = string;
  let balance = 0;
  const arr = [];

  const password = (originPassword, changePassword) => {
    if (pw !== originPassword) {
      console.log('비번이 달라요');
      return false;
    } else if (pw === originPassword) {
      pw = changePassword;
      return balance;
    }
  };
  // password변경, 기존의 pw와 첫번째 인자로 받은 originPassword와 같으면 두번째 인자로 받은 changePassword로 변경

  const unlock = password => {
    if (pw === password) {
      status = true;
      return status;
    } else {
      console.log('암호틀렸어 누구냐 너');
      status = false;
      return status;
    }
  };
  // 비밀번호 확인을 통한 잠금해제

  const lock = () => {
    status = false;
  };
  // 잠금기능

  const current = () => {
    if (status) {
      return balance;
    } else {
      console.log('잠금풀어요');
      return status;
    }
  };
  // 현재 잔액

  const charge = chargeMoney => {
    if (status) {
      if (typeof chargeMoney === 'number') {
        balance = balance + chargeMoney;
        amount = chargeMoney;
        arr.push({
          type: '충전',
          amount: chargeMoney,
          balance: balance,
        });
        return balance;
      } else {
        console.log('금액 똑바로 넣어라');
      }
    } else {
      console.log('잠금부터 풀어');
      return status;
    }
  };
  // 충전!

  const use = () => {
    if (status) {
      if (balance < 1200) {
        console.log('잔액이 부족합니다');
        return balance;
      } else {
        balance = balance - 1200;
        amount = 1200;
        arr.push({
          type: '사용',
          amount: amount,
          balance: balance,
        });
        return balance;
      }
    } else {
      console.log('잠금부터 풀어');
      return status;
    }
  };
  // 사용!

  const history = () => {
    return arr;
  };
  // 충전, 사용 내역!

  return {
    current: current,
    charge: charge,
    use: use,
    history: history,
    unlock: unlock,
    lock: lock,
    password: password,
  };
};

const myCard = createCard('jotang');

myCard.use(); // 잠금잠금
myCard.charge(1000); // 잠금잠금
myCard.unlock('1234'); // 암호틀림
myCard.unlock('jotang'); // 현재 잔액 return
myCard.charge(10000); // 10000
myCard.use(); // 8800
myCard.lock();
myCard.use(); // 잠금상태입니다
```
