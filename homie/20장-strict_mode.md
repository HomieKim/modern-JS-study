# strict mode
## strict mode 란?
- 자바스크립트 언어의 문법을 좀 더 엄격히 적용하여 오류를 발생시킬 가능성이 높거나 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시킨다. 
-  [ESLint](https://eslint.org) 같은 린트 도구를 사용해도 strict 모드와 유사한 효과를 얻을 수 있다.
- 린트 도구란 정적 분석 기능을 통해 소스코드를 실행하기 전에 소스코드를 스캔하여 문법적 오류 만이 아니라 잠재적 오류까지 찾아내고 오류의 원인을 리포팅해주는 도구이다. 
- [ESLint사용방법](https://poiemaweb.com/eslint)

## strict mode 의 적용
- 전역의 선도 또는 함수 몸체의 선두에 `'use scritct';` 키워드를 추가하여 사용
- 전역에 사용시 스크립트 전체에 `strict mode` 적용

```js
'use strict';

function foo() {
	x = 10; // ReferenceError : x is not defined
}
foo();
```

## 전역에 strict mode를 적용하는 것은 피하자
- 전역에 적용한 `strict mode`는 스크립트 단위로 적용됨
- 다른 스크립트에 영향을 주지 않기 때문에 `strict mode`와 `non strict-mode`를 혼용하는 방법은 좋지 않음
- 즉시 실행 함수로 감싸서 해결 가능

## strict mode가 발생 시키는 에러
### 암묵적 전역
- 선언하지 않은 변수를 참조하면 `Reference Error`

```js
(function () {
  'use strict';

  x = 1;
  console.log(x); // ReferenceError: x is not defined
}());
```

### 변수, 함수 ,매개변수의 삭제
- delete로 변수, 함수, 매개변수를 삭제하면 `SyntaxError`가 발생

```js
(function () {
  'use strict';

  var x = 1;
  delete x;
  // SyntaxError: Delete of an unqualified identifier in strict mode.

  function foo(a) {
    delete a;
    // SyntaxError: Delete of an unqualified identifier in strict mode.
  }
  delete foo;
  // SyntaxError: Delete of an unqualified identifier in strict mode.
}());
```

### 매개변수 이름의 중복
- 중복된 매개변수 이름을 사용하면 `SyntaxError`발생

```js
(function () {
  'use strict';

  //SyntaxError: Duplicate parameter name not allowed in this context
  function foo(x, x) {
    return x + x;
  }
  console.log(foo(1, 2));
}());
```
