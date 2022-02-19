# 20장-strict mode

## 20.1 strick mode란?
오류를 줄여 안정적인 코드 생산
js문법을 엄격하게 적용

## 20.2 strick mode 적용
```javascript
// 전역 선두에 추가시 스트립트 전체에 적용
'use strick'; 

function foo(){
    x = 10; // ReferenceError: x is not defined
}
foo();
```

```javascript
// 함수 몸체 선두에 추가시 해당 함수와 중첩 함수에 적용
function foo(){
    'use strick'; 
    x = 10; // ReferenceError: x is not defined
}
foo();
```


```javascript
// 코드의 선두에 use strick을 해야 strick mode가 작동
function foo(){
    x = 10; // 에러를 발생시키지 않음
    'use strick'; 
}
foo();
```

## 20.3 전역에 strick mode 적용 피하자
* strick, non-strick 모드를 혼용하면 오류가 날 수 있음 (외부 서드파이 라이브러리가 non-strck mode인 경우)
  
```javascript
// 즉시 실행 함수 선두에 strick mode 적용
(function(){
    'use strick';
}());
```

## 20.4 함수 단위로 strick mode 적용 피하자

## 20.5 stirck mode에서 발생하는 에러
### 20.5.1 암묵적 전역
* 선언하지 않은 변수를 참고시 ReferenceError
  
```javascript
    (function(){
        'use strick';

        x = 1;
        console.log(x); // ReferenceError: x is not defined
    }());
```

### 20.5.2 변수, 함수, 매개변수의 삭제
* 연산자 delete로 변수, 함수, 매개변수 삭제시 SyntaxError 발생
  
```javascript
(function(){
    'use strck';

    var x = 1;
    delete x; // SyntaxError: Delete of an unqualified identifier in strick mode

    function foo(a){
        delete a; // SyntaxError: Delete of an unqualified identifier in strick mode
    }

    delete foo; // SyntaxError: Delete of an unqualified identifier in strick mode

}())
``` 

### 20.5.3 매개변수 이름의 중복
* 중복된 매개변수 이름 사용시 SyntaxError

### 20.5.4 with문의 사용

## 20.6 strick mode 적용에 의한 변화
### 20.6.1 일반 함수의 this

```javascript
(function(){
    'use strick';

    function foo(){
        // 일반 함수로 호출시 this는 undefined로 바인딩
        // 일반 함수 내부에서는 this를 사용할 필요가 없기 때문
        console.log(this); // undefined
    }
    foo();

    function Foo(){
        console.log(this); // Foo
    }
    new Foo();
}())

```

### 20.6.2 arguments 객체
```javascript
(function (a){
    'use strict';
    // 매개변수에 전달된 인수를 재할당해 변경
    a=2;

    // 변경된 인수가 arguments 객체에 반영되지 않음
    console.log(arguments); // {0:1 , length:1}
}(1));
```