# 7번째 데이터 타입 Symbol

## 심벌이란?
- 자바스크립트에는 6개의 데이터 타입이 있습니다.
	- 문자열
	- 숫자
	- 불리언
	- undefined
	- null
	- 객체 타입
- 심벌(`symbol`)은 ES6에서 도입된 7번째 데이터 타입으로 변경 불가능한 원시타입의 값
- 다른 값과 중복되지 않는 유일 무이한 값으로 이름 충돌 위험이 없는 유일한 프로퍼티 키를 만들기 위해 사용한다. 
> 프로퍼티 키로 사용할 수 있는 값은 빈 문자열을 포함하는 모든 문자열 또는 심벌 값이다.

## 심벌 값의 생성

### Symbol 함수

- `Symbol`값은 다른 원시값과 다르게 리터럴 표기법이 없고 **Symbol 함수를 통해 생성합니다**
- 이때 생성된 심벌값은 외부로 노출되지 않고, **다른 값과 절대 중복되지 않는 유일무이한 값**

```js
// 함수 호출을 통해 생성
const mySymbol = Symbol();
console.log(type of my Symbol); // symbol

// 심벌 값은 외부로 노출되지 않아 확인이 불가
console.log(mySymbol); // Symbol()

// 생성자 함수처럼 보이지만
// new 연산자와 함께 사용시 객체는 생성되지만 심벌 값은 변경불가능한 원시값
new Symbol(); // TypeError : Symbol is not a constructor
```

- Symbol 함수에는 선택적으로 문자열 인수를 전달할 수 있습니다.
- 전달된 문자열은 생성된 심벌 값에대한 설명으로 디버깅 용도로만 사용됩니다.
- 심벌 값도 문자열, 숫자 , 불리언 과 같이 객체처럼 접근 시 암묵적으로 래퍼객체를 생성합니다.

```js
const mySymbol = Symbol('mySymbol');
// 객체 처럼 접근 시 래퍼 객체를 생성합니다.
console.log(mySymbol.description); // mySymbol
console.log(mySymbol.toString()); // Symbol(mySymbol)
```

- 심벌 값은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않는다.  단, 불리언 타입으로는 암묵적으로 타입 변환 된다.

```js
const mySymbol = Symbol();
// 불리언으로 암묵적 타입변환이 되므로 if문으로 존재 확인 가능
if(mySymbol) console.log('My Symbol is not empty');
```

### Symbol.for / Symbol.keyFor 메서드
#### Symbol.for
- `Symbol.for` 메서드는 인수로 전달받은 문자열을 키로 사용하여 키와 심벌 값의 쌍들이 저장되어 있는 전역 심벌 레지스트리 에서 해당 키와 일치하는 심벌 값을 검색한다.

	- 검색에 성공하면 새로운 심벌 값을 생성하지 않고 검색돤 심벌 값을 반환 한다.
	- 검색에 실패하면 새로운 심벌값을 생성하여 Symbol.for 메서드의 인수로 전달된 키로 전역 심벌 레지스트리에 저장한 후,생성된 심벌 값을 반환 한다.

```js
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
const s1 = Symbol.for('mySymbol');
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 있으면 해당 심벌 값을 반환
const s2 = Symbol.for('mySymbol');

console.log(s1 === s2); // true
```

- `Symbol.for`를 사용하면 애플리케이션 전역에서 중복되지 않는 유일 무이한 상수인 **심벌값을 단 하나만 생성하여** 전역 심벌 레지스트리를 통해 공유 할 수 있다.

#### Symbol.keyFor
- `Symbol.keyFor`  메서드를 사용하면 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출할 수 있다.

```js
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
const s1 = Symbol.for('mySymbol');
// 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출
Symbol.keyFor(s1); // -> mySymbol

// Symbol 함수를 호출하여 생성한 심벌 값은 전역 심벌 레지스트리에 등록되어 관리되지 않는다.
const s2 = Symbol('foo');
// 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출
Symbol.keyFor(s2); // -> undefined
```

## 심벌과 상수
- 상수를 정의 할 때 값에는 특별한 의미가 없고 상수 이름 자체에 의미가 있는 경우가 있다.
- 이때 문제는 상수 값이 변경되거나 다른 다른 변수 값과 중복 될 수 있다는 것
- 이런 경우 중복될 가능 성이 없는 심벌 값을 사용할 수 있다.

```js
// 위, 아래, 왼쪽, 오른쪽을 나타내는 상수를 정의한다.
// 중복될 가능성이 없는 심벌 값으로 상수 값을 생성한다.
const Direction = {
  UP: Symbol('up'),
  DOWN: Symbol('down'),
  LEFT: Symbol('left'),
  RIGHT: Symbol('right')
};

const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
  console.log('You are going UP.');
}
```

**참고**
### enum
- enum은 명명된 숫자 상수의 집합으로 열거형(`enumerated type`)이라고 부른다.
- 자바스크립트에서는 enum을 지원하지 않지만 타입스크립트나 c, java 등 다른 언어에서 지원
- 자바스크립트에서 객체 동결을 통해 객체변경을 방지해 enum을 흉내내어 사용가능

```js
const Direction = Object.freeze({
	UP : Symbol('up'),
	DOWN: Symbol('down'),
	LEFT: Symbol('left'),
	RIGHT: Symbol('right')
});
```

## 심벌과 프로퍼티 키
>  프로퍼티 키는 빈 문자열을 포함하는 모든 문자열 또는 심벌 값으로 만들 수 있다.
- 심벌 값을 프로퍼티 키로 사용하려면 프로퍼티 키로 사용할 심벌 값에 대괄호를 사용해야 한다. 
- 심벌로 생성한 프로퍼티에 접근할 때도 마찬가지로 대괄호를 사용해야 한다.

```js
const obj = {
  // 심벌 값으로 프로퍼티 키를 생성
  [Symbol.for('mySymbol')]: 1
};

obj[Symbol.for('mySymbol')]; // -> 1
```

- 심벌 값은 유일무이한 값이므로 **심벌 값으로 프로퍼티 키를 만들면 다른 프로퍼티 키와 절대 충돌하지 않는다.**

## 심벌과 프로퍼티 은닉
- 심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티는 `for ... in` 문이나 `Object.keys`, `Object.getOwnPropertyNames` 메서드로 찾을 수 없다.
- 외부에 노출할 필요가 없는 프로퍼티를 심벌 프로퍼티를 통해 은닉 할 수 있습니다. 
- ES6에서 도입된 `Object.getOwnPropertySymbols` 메서드를 사용하면 심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티를 찾을 수 있음

## 심벌과 표준 빌트인 객체 확장
- 표준 빌트인 객체를 확장하는 것은 이후 추가될 메서드의 이름과 중복될 가능성이 있으므로 권장 되지 않습니다.
- 이때 심벌 프로퍼티 키를 생성하여 표준 빌트인 객체를 확장하면 표준 빌트인 객체의 기존 프로퍼티 키와 충돌하지 않는 것은 물론, 이후 추가될 어떤 프로퍼티 키와도 충돌 위험이 없어 안전하게 표준 빌트인 객체를 확장 가능 합니다.

```js
// 심벌 값으로 프로퍼티 키를 동적 생성하면 다른 프로퍼티 키와 절대 충돌하지 않아 안전하다.
Array.prototype[Symbol.for('sum')] = function () {
  return this.reduce((acc, cur) => acc + cur, 0);
};

[1, 2][Symbol.for('sum')](); // -> 3
```

## Well-known Symbol
- 자바스크립트가 기본 제공하는 빌트인 심벌 값을 ECMAScript 사양에서는 `Well-known Symbol`이라고 부른다.
- `Well-known Symbol` 은 자바스크립트 엔진의 내부 알고리즘에 사용된다.
- 빌트인 심벌 값은 `Symbol`함수의 프로퍼티에 할당되어 있다.
- `Array`, `String`, `map`, `set` 등 `for ... of` 문으로 순회 가능한 빌트인 이터러블은 Well-known Symbol인 `Symbol.iterator`를 키로 갖는 메서드를 가진다.
- `Symbol.iterator` 메서드를 호출하면 이터레이터를 반환하도록 ECMAScript 사양에 규정되어 있다. 
- 만약 일반 객체를 이터러블 처럼 동작하도록 구현하고 싶다면 이터레이션 프로토콜을 준수하여 Symbol.iterator를 키로 갖는 메서드를 개체에 추가하고 이터레이터를 반환하도록 구현하면 됨

이처럼 심벌은 중복되지 않는 상수값을 생성하는 것은 물론 기존에 작성된 코드에 영향을 주지 않고 새로운 프로퍼티를 추가하기 위해, 즉 하위 호환성을 보장하기 위해 도입되었다.
