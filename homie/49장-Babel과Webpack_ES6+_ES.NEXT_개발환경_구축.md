# Babel과 WebPack을 이용한 ES6+/ES.NEXT 개발 환경 구축


- 매년 새롭게 도입되는 ES6이상의 버전(`ES6+`)과 제안 단계에 있는 ES 제안 사양(`ES.NEXT`)은 브라우저에 따라 지원율이 제각각
- 따라서 최신 사양으로 프로젝트를 진행하려면 구형 브라우저에서 문제 없이 동작 시키기 위한 개발환경을 구축하는 것이 필요하다.
- 대부분의 프로젝트가 모듈을 사용하므로 모듈 로더도 필요하다.
- 다음과 같은 이유로 `ESM` 보다 별도의 모듈 로더를 사용하는 것이 일반적
	1. IE를 포함한 구현 브라우저는 ESM을 지원하지 않는다.
	2. `ESM`을 사용하더라도 트랜스 파일링이나 번들링이 필요한 것은 변함 없음
	3. `ESM`이 아직 지원하지 않는 기능(bare import 등..)이 존재

## Babel
- `Babel`은 트랜스 파일링 도구로 최신 사양의 소스코드를 구형 브라우저에서도 동작하는 `ES5`사양의 코드로 변환 해주는 도구 입니다.

### 설치

```
npm install --save-dev @bable/core @babel/cli
```

### Babel 프리셋 설치와 babel.config.json 설정
- `@babel/preset-env` 설치 : 함께 사용되어야 하는 플러그인을 모아 둔 것
-   `Babel`이 제공하는 공식 프리셋(`official preset`)
    -   `@babel/preset-env`
    -   `@babel/preset-flow`
    -   `@babel/preset-react`
    -   `@babel/preset-typescript`

```
npm install --save-dev @babel/preset-env
```

- 설치후 babel.config.json 파일 생성

```js
{
  "presets": ["@babel/preset-env"]
}
```

### 트랜스파일링
- 트랜스파일링 명령어 package.json에 등록

```json
"scripts":  {
	"bulid":  "babel src/js -w -d dist/js"
},
```

- `src/js` 타겟 폴더에 있는 모든 js파일을 트랜스파일링하여 `dist/js`폴더에 저장하는 명령어
	- -w 옵션 : --watch 옵션의 축약, 타깃폴더에 있는 모든 js 파일들의 변경을 감지하여 자동으로 트랜스파일
	- -d 옵션 :  --out-dir 옵션의 축약, 트랜스파일링 결과물이 저장될 폴더를 지정, 지정된 폴더가 존재하지 않으면 자동으로 생성


### Babel 플러그인 설치
- 바벨 홈페이지에서 플러그인 검색 가능
- `@babel/plugin-proposal-class-properties`:  `public/private`  클래스 필드를 지원하는 플러그인 추가로 설치해줌

### 브라우저에서 모듈 로딩 테스트
- 브라우저는 CommonJS  방식의 require 함수를 지원하지 않음으로 별도의 설정없이 실행하면 에러남
- 노드 모듈이 아니라 브라우저의 ES6 모듈을 사용하도록 Babel을 설정할 수도 있으나 Webpack을 통해 해결하는 것이 좋음

## Webpack

- `Webpack`은 의존 관계에 있는 자바스크립트, CSS, 이미지 등의 리소스들을 하나의 파일로 번들링 하는 모듈 번들러
- `Webpack`을 사용하면 의존 모듈이 하나의 파일로 번들링 되므로 별도의 모듈로더가 필요 없다.
- HTML 파일에서 여러개의 script 태그로 자바스크립트 파일을 로드해야하는 번거로움도 사라진다.

### babel-loader 설치
- `Webpack`이 모듈을 번들링할 때  `Babel`을 사용하여  `ES6+`/`ES.NEXT`  사양의 소스코드를  `ES5`  사양의 소스코드로 트랜스파일링하도록  `babel-loader`  설치.

```
npm install --save-dev babel-loader
```

### webpack.config.js  설정 파일 작성

```js
const path = require('path');

module.exports = {
  entry: './src/js/main.js',
  output:{
    path: path.resolve(__dirname, 'dist'),
    filename: 'js/bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        include: [path.resolve(__dirname, "src/js")],
        exclude: /node_modules/,
        use:{
          loader: "babel-loader",
          options: {
            presets: ['@babel/preset-env'],
            plugins: ['@babel/plugin-proposal-class-properties']
          }
        }
      }
    ]
  },
  devtool: 'source-map',
  mode: 'development'
}
```

### babel-polyfill 설치
- Babel을 사용하여 최신 사양의 코드를 ES5 사양의 코드로 트랜스파일링해도 브라우저가 지원하지 않는 코드가 남아있을 수 있다.
-   `Promise`,  `Object.assign`,  `Array.from`  등과 같이  `ES5`  사양으로 대체할 수 없는 기능은 트랜스파일링 되지 않음.
-   `IE`  같은 구형 브라우저에서도 위와 같은 객체나 메서드를 사용하기 위해서  `@babel/polyfill`  설치.

> @babel-polyfill은 개발 환경에서만 사용하는 것이라 실제 운영 환경 에서도 사용해야 하므로 --save-dev 옵션 지정하지 않음

-   `ES6`의  `import`를 사용하는 경우 진입점의 선두에서 폴리필을 로드

```js
import "@babel/polyfill";
import { pi, power, Foo } from "./lib";
```    
 
-   `Webpack`을 사용하는 경우  `webpack.config.js`  파일의  `entry`  배열에 폴리필을 추가.

```js
module.exports = {
	entry: ["@babel/polyfill", "./src/js/main.js"],
};
```
    
