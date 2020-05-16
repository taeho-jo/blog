---
title: 'webpack으로 React개발환경 세팅하기'
date: '2020-05-16'
category: 'webpack'
---

# webpack
모듈 번들러란, 여러개의 나누어져 있는 파일들을 하나의 파일로 만들어주는 라이브러리를 말한다. 그리고 그 모듈 번들러 라이브러리 중 하나가 **웹팩(webpack)**이다.
![](https://images.velog.io/images/jotang/post/d6fb9ff1-7798-4ad9-ad4e-0b37840e55ac/image.png)

## 모듈 번들러
예전 모듈 번들러라는 개념이 없던 시절이라 생각하고, 웹페이지에 접속을 하게 된다면 웹페이지는 페이지를 보여주기 위해서 많은 양의 자바스크립트 파일을 서버에 요청하게 된다.
또한, 모듈 개념이 없었기 때문에 파일이 나누어져 있어도 변수 스코프를 생각하면서 개발을 진행해야 했다.

이러한 문제를 해결하기 위해서 모듈의 개념이 생겨났고, 이러한 모듈을 지원하지 않는 브라우저들이 있기 때문에, 브라우저들이 지원할 수 있는 코드로 변환을 해주어야한다. 이러한 것을 지원해 주는 라이브러리가 **웹팩(webpack), Parcel** 등 인 것이다.

결국 모듈 번들러는 **여러개의 자바스크립트 파일을 하나로 뭉쳐서 한 번의 요청에 가지고 올 수 있게 도와주고, 최신 자바스크립트 문법을 브라우저에서 사용할 수 있게** 해준다.

## 웹팩(webpack)의 주요 4가지 개념
### 1. entry(엔트리)
웹팩에서 모든 것은 모듈이다. 자바스크립트, 스타일시트, 이미지 등 모든 것을 자바스크립트 모듈로 로딩해서 사용하도록 하는 것이다.

아래 그림처럼 자바스크립트가 로딩하는 모듈이 많아질수록 모듈간 의존성은 증가할 수 밖에 없다. 저 그림에서 의존성 **그래프의 시작점이 되는 곳**을 웹팩에서 **entry(엔트리)**라고 한다.
![](https://images.velog.io/images/jotang/post/9ef2e7e4-83cd-4ced-a3ac-33f510ee6f46/image.png)

엔트리를 통해서 필요한 모든 모듈들을 로딩한 후 하나의 파일로 묶는다.

```js
module.exports = {
  entry: "./src/index.js'
}
```
html에서 사용할 자바스크립트의 시작점은 src/index.js 파일이다. src/index.js가 결국 entry인 것이다.

### 2. output(아웃풋)
엔트리에서 설정한 자바스크립트 파일을 시작으로 의존되어 있는 모든 모듈을 하나로 묶은 후에, **번들된 결과물을 처리할 위치**를 output에 작성한다.
```js
module.exports = {
  output: {
    filename: 'index.js',
    path: './build'
  }
}
```

build폴더에 index.js파일로 저장하게 된다.
html에서 번들링된 index.js를 불러오게 한다.
```html
<body>
  <script src='./build/index.js'></script>
</body>
```

### 3. loader(로더)
웹팩은 자바스크립트 밖에 알지 못한다. 따라서 이미지, 폰트, 스타일시트 등 **비 자바스크립트 파일을 웹팩이 이해하게끔 변경**하는 역할을 로더가 한다.

로더는 test와 use키로 구성된 객체로 설정한다.
* test: 로딩할 파일을 지정
* use: 적용할 로더를 지정

#### babel-loader
babel-loader는 es6를 es5로 변환할 때 사용한다. test에 es6로 작성된 자바스크립트 파일의 경로를 작성하고, use로 이를 변환시켜 줄 바벨로더를 설정하면 된다.
```js
module.exports = {
  module: {
    rules: [{
      text: /\.js$/,
      exclude: 'node_modules',
      use: {
        loader: ['babel-loader'],
        options: {
          presets: ['env']
        }
      }
    }]
  }
}
```

#### css-loader, style-loader
css 파일도 자바스크립트로 변환한 후 로딩애햐한다. 그 역할을 하는 것이 **css-loader**.
```js
// module
exports.push([module.i, "body {\n  background-color: green;\n}\n", ""]);
```

이렇게 변경되고 나면 돔에 추가되어야지만 브라우저가 해석을 할 수 있다. 스타일시트를 동적으로 돔에 추가해주는 로더가 **style-loader**. 보통 두 가지를 함께 사용한다.
```js
module.exports = {
  module: {
    rules: [{
      test: /\.css$/,
      use: ['style-loader', 'css-loader']
    }]
  }
};
```

### 4. plugin(플러그인)
로더가 파일단위로 처리한다면, 플러그인은 번들된 결과물을 처리한다.

번들된 자바스크립트를 난독화 하거나, 특정 텍스트를 추출하는 용도로 사용하게 된다.

#### UglifyJsPlugin
난독화를 처리하는 플러그인.
플러그인을 사용할 때는 웹팩 설정 객체의 plugin 배열에 추가한다.
```js
const webpack = require('webpack')

module.exports = {
  plugins: [
    new webpack.optimize.UglifyJsPlugin(),
  ]
}
```

조금 더 자세한 사항은 아래에서 React의 초기 개발환경을 구축하면서 진행해보려고 한다.

# React와 webpack

## 프로젝트 생성
먼저 프로젝트를 진행할 폴더를 하나 생성한 후에,
`npm init` 를 통해 초기화를 진행한다.

### react 
`npm install --save-dev react react-dom`


### babel
`npm install --save-dev @babel/core @babel/preset-env @babel/preset-react babel-loader`

#### @babel/core
리액트는 es6를 사용하기 때문에, 여러 브라우저에서 사용이 가능하도록 es5문법으로 바꿔준다

#### @babel/preset-env
es6를 es5로 바꿔주고, es6뿐만 아니라 브라우저에 따라 알아서 컴파일을 진행해 준다.
[@babel/preset-env 참고글](https://velog.io/@pop8682/%EB%B2%88%EC%97%AD-%EC%99%9C-babel-preset%EC%9D%B4-%ED%95%84%EC%9A%94%ED%95%98%EA%B3%A0-%EC%99%9C-%ED%95%84%EC%9A%94%ED%95%9C%EA%B0%80-yhk03drm7q)

#### @babel/preset-react
리액트에서는 jsx를 사용하는데 자바스크립트로 바꿔준다.

#### babel-loader
자바스크립트 파일을 babel preset/plgins과 webpack을 사용하여 es5로 컴파일 해주는 plugin이다.

### webpack
`npm install --save-dev webpack webpack-dev-server webpack-cli html-loader css-loader node-sass sass-loader html-webpack-plugin mini-css-extract-plugin    
`

#### webpack
모든 리액트 파일을 하나의 컴파일된 하나의 자바스크립트 파일에 넣는 역할

#### webpack-dev-server
개발환경에서 바로 확인을 할 수 있다 (live-reload)
 
#### webpack-cli
build 스크립트를 통해 webpack 커맨드를 사용하기 위해서 

#### html-loader, css-loader, sass-loader
html, css, sass를 위한 로더

#### html-webpack-plugin, mini-css-extract-plugin
html과 css를 위한 플러그인

## HTML build
최상위 폴더에서 webpack.config.js 파일을 생성하고 아래의 코드를 작성한다.
```js
const path = require('path')

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'index.js',
    path: path.resolve(__dirname + '/build')
  },
  mode: 'none'
};
```
entry는 웹팩이 빌드할 파일을 알려주는 역할을 하게 된다. src/index.js 파일을 기준으로 import 되어있는 모든 파일들을 하나의 파일로 합쳐주게된다.

output은 웹팩에서 빌드를 완료하면 output에 명시한 정보를 통해 빌드 파일을 생성한다.

mode는 웹팩 빌드 옵션, production, development에 대해서는 아래에서 더 자세히 알아보도록 하겠다. none은 아무런 기능 없이 웹팩으로 빌드하게 된다.

package.json의 script에 추가한다
```json
...
"script" : {
  "build": "webpack"
}
...
```

이제 로더를 사용하여 html을 웹팩으로 빌드를 해보려고한다!

먼저 **로더를 작성하는 법**은 아래와 같다
```js
module: {
  rules: {
    test: '가지고 올 파일 정규식',
    use: [
      {
        loader: '사용할 로더, 여러개라면 배열로 작성',
        options: { 사용할 로더의 옵션 }
      }
    ]
  }
}
```

먼저 public 폴더를 생성한 후에 index.html 파일을 만든다
```html
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="utf-8" />
    <title>webpack-react</title>
  </head>
  <body>
    <div id="root"></div>
  </body>
</html>
```

html 파일을 생성한 후에 webpack.config.js에 html 관련 코드를 작성한다

```js
const path = require('path');
const HtmlWebPackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'index.js',
    path: path.resolve(__dirname + '/build')
  },
  mode: 'none',
  module: {
    rules: [
      {
        test: /\.html$/,
        use: [
          {
            loader: 'html-loader',
            options: { minimize: true }
          }
        ]
      }
    ]
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: './public/index.html',
      filename: 'index.html'
    })
  ]
};
```
**html-webpack-plugin**은 웹팩이 html을 읽고, 그 파일을 빌드할 수 있게 해준다.
options에 있는 minimize는 코드 최적화 옵션으로 파일 내용을 한줄로 빌드해준다.

**templatae**는 작성된 경로에 있는 파일을 읽는다.
**filname**은 output으로 출력할 파일을 말한다.

## react build
react를 빌드하는 것도 비슷하게 작업을 진행하면 된다.
먼저 src/index.js파일을 생성하고 코드를 작성한다.
```js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

ReactDOM.render(<App />, document.getElementById('root'));
```

그리고는 src폴더에 App.js파일을 생성하도록하자
```js
import React from 'react'

const App = () => {
  return (
    <h1>Hello, React</h1>
  );
};

export defalut App;
```

최상위 폴더에서 `.babelrc` 파일을 생성하고, 아래 코드를 작성한다
```json
{
  "presets": ["@babel/preset-env", "@babel/preset-react"]
}
```
위에서도 살펴보았지만 바벨은 es6를 es5로 변환해주는 역할을 하는데, 위의 코드는 바벨이 es6와 리액트를 es5로 변환할 수 있도록 해준다.

이제 webpack.config.js에서 entry와 babel-loader를 추가한다.

```js
const path = require('path');
const HtmlWebPackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'index.js',
    path: path.resolve(__dirname + '/build')
  },
  mode: 'none',
  module: {
    rules: [
      {
        test: /\.html$/,
        use: [
          {
            loader: 'html-loader',
            options: { minimize: true }
          }
        ]
      },
      {
        test: /\.(js|jsx)$/,
        exclude: '/node_modules', //제외
        use: ['babel-loader']
      }
    ]
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: './public/index.html',
      filename: 'index.html'
    })
  ]
};
```
npm run build 후에 확인해보면 Hello, React가 출력되는 것을 확인 할 수 있을 것이다.

## css, scss build
src 폴더에 style.css를 추가하고, 간단한 스타일을 적용한다

```css
.title {
    font-size: 30px;
    color: palevioletred;
    font-weight: bold;
}
```
```js
import React from 'react'

const App = () => {
  return (
    <>
      <h1>Hello, React</h1>
      <p className='title'>안녕, css-loader</p>
    </>
  );
};

export defalut App;
```
이 상태로 빌드를 하게되면 error가 난다. css-loader를 추가해보자

```js
const path = require('path');
const HtmlWebPackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'index.js',
    path: path.resolve(__dirname + '/build')
  },
  mode: 'none',
  module: {
    rules: [
      {
        test: /\.html$/,
        use: [
          {
            loader: 'html-loader',
            options: { minimize: true }
          }
        ]
      },
      {
        test: /\.(js|jsx)$/,
        exclude: '/node_modules', //제외
        use: ['babel-loader']
      },
      {
        text: /\.css$/,
        use: [MiniCssExtractPlugin.loader, 'css-loader']
        // use에 있는 loader는 오른쪽에서 왼쪽의 순서로 실행된다.
        // css-loader로 css파일을 읽고, 
        // MiniCssExtractPlugin.loader를 통해 css파일을 추출
      }
    ]
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: './public/index.html',
      filename: 'index.html'
    }),
    new MiniCssExtractPlugin({
      filename: 'style.css'
    })
  ]
};
```
mini-css-extract-plugin을 사용하지않고 css-loader만 사용하게 되면, 빌드는 완료되지만 css가 적용되어 있지는 않다. css파일을 읽기만하고 저장을 하지 않았기 때문이다. 따라서 css-loader로 css파일을 읽고, **css파일을 추출하여 저장**하기 위해서 mini-css-extract-plugin도 함께 사용한다.

scss도 css와 크게 다르지 않다.

```js
const path = require('path');
const HtmlWebPackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'index.js',
    path: path.resolve(__dirname + '/build')
  },
  mode: 'none',
  module: {
    rules: [
      {
        test: /\.html$/,
        use: [
          {
            loader: 'html-loader',
            options: { minimize: true }
          }
        ]
      },
      {
        test: /\.(js|jsx)$/,
        exclude: '/node_modules', //제외
        use: ['babel-loader']
      },
      {
        text: /\.css$/,
        use: [MiniCssExtractPlugin.loader, 'css-loader']
      },
      {
        text: /\.scss$/,
        use: [MiniCssExtractPlugin.loader, 'css-loader', 'sass-loader]
      }
    ]
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: './public/index.html',
      filename: 'index.html'
    }),
    new MiniCssExtractPlugin({
      filename: 'style.css'
    })
  ]
};
```

## dev server 
지금까지는 코드를 수정할 때마다 웹팩으로 직접 빌드를 해야했다.
코드가 수정될 때 웹팩이 알아서 빌드해주는 것이 webpack-dev-server이다.

적용하는 방법은 매우 간단하다.
```js
const path = require('path');
const HtmlWebPackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'index.js',
    path: path.resolve(__dirname + '/build')
  },
  devServer: {
    contentBase: path.resolve('./build'),
    index: 'index.html',
    port: 9000
  },
  mode: 'none',
  module: {
    ...
  },
  plugin: [
    ...
  ]
```

package.json의 script에도 추가해준다

```json
"script" : {
  "build": "webpack",
  "start": "webapck-dev-server --hot"
}
```
이후 npm run start를 통해 localhost:9000에 접속하면 빌드된 모습을 확인할 수 있다.


## build 폴더 정리하기
clean-webpack-plugin는 빌드가 될 때 마다 사용하지 않는 파일들을 자동으로 삭제하여 폴더를 관리해준다.

먼저 css의 filename을 index.css로 변경한다.
```js
plugins: [
	new HtmlWebPackPlugin({
		template: './public/index.html', // public/index.html 파일을 읽는다.
		filename: 'index.html' // output으로 출력할 파일은 index.html 이다.
	}),
	new MiniCssExtractPlugin({
		filename: 'index.css'
	})
]
```
```js
const { CleanWebpackPlugin } = require("clean-webpack-plugin");

module.exports = {
 	...,
  plugins: [
    new HtmlWebPackPlugin({
      template: './public/index.html', // public/index.html 파일을 읽는다.
      filename: 'index.html' // output으로 출력할 파일은 index.html 이다.
    }),
    new MiniCssExtractPlugin({
      filename: 'index.css'
    }),
    new CleanWebpackPlugin()
  ]
};
```
플러그인에 추가후에 실행하면 사용하지 않는 파일이 삭제되는 것을 볼 수 있다.

## webpack build mode 나누기
웹팩은 빌드 모드가 development와 production 두가지가 있고, 차이가 있다.

간단하게,
* **development** : 빠른 빌드를 위해서 최적화를 하지 않는다는 특성을 지닌다.
* **production** : 빌드할 때 최적화 작업을 진행한다.

config 폴더를 생성하고, webpack.config.dev.js 와 webpack.config.prod.js를 만들고 똑같이 코드를 작성하면 되지만, 
dev.js에는 `mode: 'development'`, prod.js에서는 `mode: 'production'` 으로 작성해준다.

그리고 package.json의 script를 수정해준다
```json
"scripts": {
  "start": "webpack-dev-server --config ./config/webpack.config.dev --hot",
  "build": "webpack --config ./config/webpack.config.prod"
 },
````

### 끝
다른 블로그의 도움을 받아서 react에 개발환경을 설정하면서 webpack에 대해서 공부하였습니다. 여러번 따라하고 혼자하고 하다보니 조금 더 이해가 되는 것 같습니다. loader와 plugin이 어떠한 역할을 하는지 알게되었고, 웹팩이 이런거구나에 대해서 흐름을 알게되었습니다.

깊은 지식이 있는 글은 아니지만, 이제는 다른 것들을 하나씩 붙여가며 시도할 수 있을 것 같습니다. 추후에 또 webpack을 하게 되었을 때, 또 업데이트 하도록 하겠습니다.

**참고 블로그**
[참고 블로그](https://velog.io/@jeff0720/React-%EA%B0%9C%EB%B0%9C-%ED%99%98%EA%B2%BD%EC%9D%84-%EA%B5%AC%EC%B6%95%ED%95%98%EB%A9%B4%EC%84%9C-%EB%B0%B0%EC%9A%B0%EB%8A%94-Webpack-%EA%B8%B0%EC%B4%88#%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0)