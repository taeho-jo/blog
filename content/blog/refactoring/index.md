---
title: 'Refactoring'
date: '2020-03-26'
category: 'Refactoring'
---

# Refactoring 이란?
외부 동작, 즉 **기능에 대한 변경없이 코드를 개선**하는 것이다.
결국 리팩토링의 목적은 코드의 가독성을 좋게하고, 그에 따라 유지보수가 쉽도록 하는 것이다.

리팩토링을 진행할 때에는 외부 동작이 바뀌지 않았다는 것을 검증해야한다.
그렇기 때문에 검증을 위해서 테스트 코드가 필요하다.

### Legacy Code Refactoring
#### Leagcy Code란?
간단하게 설명해서 레거시 코드란 이전에 이미 작업되어 있던 코드들을 말한다. 다른 `누군가로 부터 이어받은 코드` 라고 정의할 수 있다.

> Michael Feathers는 "Code Without Test" 라고 하였다. (테스트코드가 없는 코드)

![](https://images.velog.io/images/jotang/post/a02aa3cd-a525-40af-8e9f-f13911453ff9/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-03-26%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.40.08.png)

![](https://images.velog.io/images/jotang/post/f42d4ffc-0591-4576-8930-1b65e8160ebe/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-03-26%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.41.23.png)

처음 그림처럼 한 번에 모든 것을 리팩토링하는 것이아니고, 두번째 그림처럼 한 단계씩 천천히 테스트를 진행해 가면서 리팩토링을 진행한다.

# Bad Smell
리팩토링에 앞서 이러한 코드들을 리팩토링하려고 노력해야한다.

자세한 것은 차차 공부해 나가면서 알아가도록 하겠다. 오늘은 관련 자료 링크를 투척한다.

[Bad Smell](https://nesoy.github.io/articles/2018-05/Refactoring-BadSmell)

# Characterization Test
### Characterizstion Test 란?
어떠한 코드를 리팩토링을 하기 위해서는 해당 코드의 동작을 알고있어야 하는데, 알지 못하는 상황이라면 리팩토링을 진행할 수 가 없다.

정확한 동작을 모르는 코드를 수정 해야 할 때에 테스트 코드를 사용할 수 있는데 그 방법이 Characterization Test이다.

일반적인 테스트와 다르게 동작의 옳고 그름을 검증한다기 보다는 **동작 그자체를 이해하기 위해 사용하는 테스트**라고 할 수 있다.

### 간단한 실습을 진행해 보도록 하겠습니다.

실습으로 리팩토링 할 statement 는 `invoice`와 `plays` 정보를 이용하여 Bill 정보를 출력하는 함수입니다. 아래 내용은 해당 입력과 출력에 대한 샘플 데이터입니다.
```js
// invoice

{
  "customer": "BigCo",
  "performances": [
    {
      "playID": "hamlet",
      "audience": 20
    },
    {
      "playID": "asLike",
      "audience": 15
    }
  ]
}
```

```js
// plays

{
  "hamlet": {"name": "Hamlet", "type": "tragedy"},
  "asLike": {"name": "As You Like It", "type": "comedy"}
}
```

```js
function statement (invoice, plays) {
    let totalAmount = 0;
    let volumeCredits = 0;
    let result = `Statement for ${invoice.customer}\n`;
    const format = new Intl.NumberFormat("en-US",
        { style: "currency", currency: "USD",
            minimumFractionDigits: 2 }).format;
    for (let perf of invoice.performances) {
        const play = plays[perf.playID];
        let thisAmount = 0;

        switch (play.type) {
            case "tragedy":
                thisAmount = 40000;
                if (perf.audience > 30) {
                    thisAmount += 1000 * (perf.audience - 30);
                }
                break;
            case "comedy":
                thisAmount = 30000;
                if (perf.audience > 20) {
                    thisAmount += 10000 + 500 * (perf.audience - 20);
                }
                thisAmount += 300 * perf.audience;
                break;
            default:
                throw new Error(`unknown type: ${play.type}`);
        }

        // add volume credits
        volumeCredits += Math.max(perf.audience - 30, 0);
        // add extra credit for every ten comedy attendees
        if ("comedy" === play.type) volumeCredits += Math.floor(perf.audience / 5);

        // print line for this order
        result += `  ${play.name}: ${format(thisAmount/100)} (${perf.audience} seats)\n`;
        totalAmount += thisAmount;
    }
    result += `Amount owed is ${format(totalAmount/100)}\n`;
    result += `You earned ${volumeCredits} credits\n`;
    return result;
}

module.exports = statement;
```

### 테스트코드 작성!!
test라는 폴더를 생성 한 후, `statement.test.js` 파일을 생성한 후, 가장 기본적인 테스트를 작성한다.

```js
const assert = require('assert');
const statement = require('../src/statement');

describe('statement', () => {
  it('trivial', () => {
    assert("", "");
  });
});
```

- 테스트 코드는 `describe()`으로 테스트 suite을 만들고 그 안에서 `it`으로 테스트 코드를 작성한다.

- `describe()`는 중첩해서 사용할 수 있다.

- 본 테스트코드는 WebStorm에서 진행하였다.

- statement.test.js를 우클릭하면 테스트코드를 실행할 수 있다.

![](https://images.velog.io/images/jotang/post/3e891658-8319-476c-8dcd-4f4d00fd1b94/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-03-26%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%206.04.16.png)

- Coverage는 테스트가 얼마나 진행되었는지 알려준다. 퍼센트로도 알려주고, 코드가 작성된 왼쪽에 초록줄과 빨간줄로 얼만큼 진행되었는지 알려준다.


#### 먼저 완성된 테스트 코드
```js
const assert = require('assert');
const statement = require('../src/statement');

describe('statement', () => {
    it('for empty performances', () => {
        let invoice = {
            customer: 'BigCo',
            performances: []
        };
        let plays;
        const result = statement(invoice, plays);
        assert.strictEqual(result, "Statement for BigCo\n" +
                                    "Amount owed is $0.00\n" +
                                     "You earned 0 credits\n");
    });
    it('for one performance with less than 30 audience', () => {
        let invoice = {
            customer: 'BigCo',
            performances: [
                {
                    playID: 'hamlet',
                    audience: 20
                },
            ]
        };
        let plays = {
            hamlet: { name: 'Hamlet', type: 'tragedy' }
        };
        const result = statement(invoice, plays);
        assert.strictEqual(result, "Statement for BigCo\n" +
            "  Hamlet: $400.00 (20 seats)\n" +
            "Amount owed is $400.00\n" +
            "You earned 0 credits\n");
    });
    it('for comedy with 20 audience', () => {
        let invoice = {
            customer: 'BigCo',
            performances: [
                {
                    playID: 'asLike',
                    audience: 20
                },
            ]
        };
        let plays = {
            asLike: { name: "As You Like It", type: "comedy" }
        };
        const result = statement(invoice, plays);
        assert.strictEqual(result, "Statement for BigCo\n" +
            "  As You Like It: $360.00 (20 seats)\n" +
            "Amount owed is $360.00\n" +
            "You earned 4 credits\n");
    });

    it('for comedy with more than 20 audience', () => {
        let invoice = {
            customer: 'BigCo',
            performances: [
                {
                    playID: 'asLike',
                    audience: 21
                },
            ]
        };
        let plays = {
            asLike: { name: "As You Like It", type: "comedy" }
        };
        const result = statement(invoice, plays);
        assert.strictEqual(result, "Statement for BigCo\n" +
            "  As You Like It: $468.00 (21 seats)\n" +
            "Amount owed is $468.00\n" +
            "You earned 4 credits\n");
    });
    it('for unknown type', () => {
        let invoice = {
            customer: 'BigCo',
            performances: [
                {
                    playID: 'asLike',
                    audience: 21
                },
            ]
        };
        let plays = {
            asLike: { name: "As You Like It", type: "unknown" }
        };
        assert.throws(() => {
                const result = statement(invoice, plays);
            }, /^Error: unknown type: unknown$/
        );
    });
    it('for multiple performances', () => {
        let invoice = {
            customer: 'BigCo',
            performances: [
                {
                    playID: 'hamlet',
                    audience: 31
                },
                {
                    playID: 'asLike',
                    audience: 21
                }
            ]
        };
        let plays = {
            hamlet: { name: 'Hamlet', type: 'tragedy' },
            asLike: { name: "As You Like It", type: "comedy" }
        };
        const result = statement(invoice, plays);
        assert.strictEqual(result, "Statement for BigCo\n" +
            "  Hamlet: $410.00 (31 seats)\n" +
            "  As You Like It: $468.00 (21 seats)\n" +
            "Amount owed is $878.00\n" +
            "You earned 5 credits\n");
    });
});
```

테스트 코드를 작성하는 순서는 아래와 같다

어떻게 작동하는 코드인지 알 수 없기 때문에, x라는 임의 이름을 가진 테스트로 시작을 한다.
```js
it('x', () => {
  let invoice;
  let plays;
  const result = statement(invoice, plays);
  assert.strictEqual(result, "");
});
```

테스트를 실행하게 되면 물론 error가 발생하게 된다. 이때 샘플 데이터들을 참고해가면서 error를 업애주면된다.
```js
it('x', () => {
  let invoice = {
    customer: 'BigCo',
    performances: []
  };
  let plays;
  const result = statement(invoice, plays);
  assert.strictEqual(result, "");
});
```

`assert.strictEqual(result,"")` 이 부분은 내가 예상한 결과 값과 테스트를 진행하였을 때 나오는 결과 값을 비교하게 된다.
- result는 실제로 나오는 결과 값을 담고, ""에는 내가 예상한 결과 값을 넣게 된다.

위의 코드를 테스트하게 되면, Assertion 에러만 나고, 이 때 나온 결과 값을 복사하여 ""에 넣어준다.


테스트 통과
```js
it('x', () => {
  let invoice = {
    customer: 'BigCo',
    performances: []
  };
  let plays;
  const result = statement(invoice, plays);
  assert.strictEqual(result, "Statement for BigCo\n" +
    "Amount owed is $0.00\n" +
    "You earned 0 credits\n");
});
```

`invoice.performances`가 empty인 경우에 대해 코드의 동작을 확인 하였다.
x로 지정해주었던 테스트코드 이름도 적절한 이름으로 변경해 주면된다.

이러한 순서로 작성된 코드를 따라 테스트를 진행하면 된다
