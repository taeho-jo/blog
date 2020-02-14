---
title: 'Next.js Router'
date: '2020-01-15'
category: 'next.js'
---

# Next Router

- Next.js는 React Router를 사용하지 않고, 전용 Router를 사용한다. SSR등을 포함하여 단일한 API를 제공하기 위해서이다.
- next/link 와 next/router 두 가지만 기억하면 된다.

## next/link

- default로 Link 객체를 import해서 사용된다.
- React component로써 JSX를 이용한 고레발 API를 제공하여 라우팅을 지원한다

```jsx
// next/link 사용 예시 1: 기본
// components/menu-item.js
import React from 'react';
import Link from 'next/link';

export default ({ children, href }) => (
  <li
    style={{
      marginRight: '10px',
      color: '#ddd',
      padding: '5px',
    }}
  >
    <Link href={href}>
      <a>{children}</a>
    </Link>
  </li>
);
```

```jsx
// next/link 사용 예시 2: pushState 대신 replaceState
// components/menu-item.js
import React from 'react';
import Link from 'next/link';

export default ({ children, href }) => (
  <li
    style={{
      marginRight: '10px',
      color: '#ddd',
      padding: '5px',
    }}
  >
    <Link href={href} replace>
      <a>{children}</a>
    </Link>
  </li>
);
```

```jsx
// next/link 사용 예시 3: querystring
// components/menu-item.js
import React from 'react';
import Link from 'next/link';

export default ({ children, href, querystrings }) => (
  <li
    style={{
      marginRight: '10px',
      color: '#ddd',
      padding: '5px',
    }}
  >
    <Link
      href={{
        pathname: href,
        query: {
          querystring1: querystrings[0],
          querystring2: querystrings[1],
          //...
        },
      }}
    >
      <a>{children}</a>
    </Link>
  </li>
);
```

## next/router

- Router 객체를 import해서 사용한다.
- 프로그래머에게 제어권이 있는 Programmable API를 제공한다.

### router 속성과 method

- **Router.route**: 현재 경로 반환

- **Router.pathname**: queryString 을 포함한 전체 경로 반환

- **Router.query**: queryString이 파싱되어 저장된 객체로 빈 값은 {}로 반환

- **Router.push(url, as=url)**: 주어진 url 파라미터에 따라 pushState() 메서드를 호출

- **Router.replace(url, as=url)**: 주어진 url 파라미터에 따라 replaceStete() 메서드 호출

```jsx
// next/router 사용 예시 1: 기본
// components/custom-link.js
import React from 'react';
import Router from 'next/router';

export default ({ className, children, href }) => (
  <li className={className}>
    <span onClick={() => Router.push(href)}>{children}</span>
  </li>
);
```

### router의 event 종류

- **routeChangeStart(url)**: 라우팅이 시작될 때 호출

- **routeChangeComplete(url)**: 라우팅이 끝나면 호출

- **routeChangeError(err, url)**: 라우팅 도중 에러 발생 시 호출

- **beforeHistoryChange(url)**: 브라우저 내의 히스토리가 바뀌기 직전에 호출

- **appUpdated(nextRoute)**: 페이지가 업데이트 되었는 데 새 버전의 어플리케이션이 사용 가능한 경우 호출

```jsx
// next/router 사용 예시 2: 이벤트 리스너 등록
// pages/about.js

// ~~ ...

Router.onRouteChangeStart = url => {
  console.log(`Route가 ${url}로 변경될 것입니다...!`);
};
Router.onRouterChangeError = (err, url) => {
  if (err) {
    console.error(`Route를 ${url}로 변경 도중 에러가 발생하였습니다...!`);
    console.dir(err);
  }
};
```
