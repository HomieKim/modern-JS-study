# 제너레이터와 async/await

## 제너레이터란?
- ES6에서 도입 된 제너레이터는 코드 블록의 실행을 일시 중지 했다가 필요한 시점에 재개할 수 있는 특수한 함수 입니다.
- 일반 함수와의 차이

1. 제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다.
	- 이때 함수의 제어권을 호출자에게 넘기는 것을 양도(`yield`)라고 함

2. 제너레이터 함수는 함수 호출자와 함수의 상태를 주고 받을 수 있다.
	- 제너레이터 함수는 함수 호출자에게 상태를 전달할 수 있고 함수 호출자로 부터 상태를 전달 받을 수도 있다.

3. 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
	- 제너레이터 함수를 호출하면 함수 코드를 실행하는 것이 아니라 이터러블이면서 동시에 이터레이터인 제너레이터 객체를 반환한다.

## 제너레이터 함수의 정의
- 제너레이터 함수는 `function*` 키워드로 정의
- 하나 이상의 `yield` 표현식을 포함한다.
-  제너레이터 함수는 화살표 함수로 정의할 수 없다.
- 제너레이터 함수는 `new` 연산자와 함께 생성자 함수로 호출할 수 없다.

```js
// 제너레이터 함수 선언문
function* genDecFunc() {
  yield 1;
}

// 제너레이터 함수 표현식
const genExpFunc = function* () {
  yield 1;
};

// 제너레이터 메서드
const obj = {
  * genObjMethod() {
    yield 1;
  }
};

// 제너레이터 클래스 메서드
class MyClass {
  * genClsMethod() {
    yield 1;
  }
}
```

## 제너레이터 객체
- 제너레이터 함수를 호출하면 일반 함수처럼 함수 코드 블록을 실행하는 것이 아니라 **제너레이터 객체를 생성해 반환**
- 제너레이터 객체는 **이터러블 이면서 동시에 이터레이터**
- 이터레이터 이므로 `next()` 메서드를 가짐
- 다른 이터레이터와 다르게 `return`, `throw` 메서드를 가진다.
- 메서드 동작 방식
	-  `next` :  `yield` 표현식 까지 코드 블록을 실행하고 `yield` 된 값을 `value` 프로퍼티 값으로, `false` 값을 갖는 `done`프로퍼티를 가진 리절트 객체를 반환
	- `return` :  인수로 전달 받은 값을 `value` 프로퍼티 값으로, `done` 프로퍼티 값이 `true` 인 리절트 객체 반환
	- `throw`  :  인수로 전달 받은 에러를 발생시키고 `undefined` 값을 `value` 프로퍼티의 값으로, `done` 프로퍼티의 값이 true인 리절트 객체 반환

```js
function* genFunc() {
  try {
    yield 1;
    yield 2;
    yield 3;
  } catch (e) {
    console.error(e);
  }
}

const generator = genFunc();

console.log(generator.next()); // {value: 1, done: false}
// return 했을 때
console.log(geneator.return('End!')); // {value: "End!", done: true}
// throw 했을 때
console.log(generator.throw('Error!')); // {value: undefined, done: true}
```

## 제너레이터의 일시 중지와 재개
- 제너레이터 함수는 일반함수처럼 코드 블록을 일괄 실행하는 것이 아니라 `yield` 표현식 까지만 실행
- `yield` 키워드는 제너레이터 함수의 실행을 일시 중지 시키거나 `yield` 키워드 뒤에 오는 표현식의 평가 결과를 제너레이터 함수 호출자에게 반환

```js
function* genFunc(){
	yield 1;
	yield 2;
	yield 3;
}

// 제너레이터 함수 실행시 제너레이터 객체를 반환합니다.
const generator = genFunc();
// next 메서드를 호출하면 yield 표현식 까지 실행 되고 일시중지됩니다.
// 이때 함수의 제어권이 호출자로 양도(yield) 됩니다.
console.log(generator.next()); // { value: 1, done : false}
console.log(generator.next()); // { value: 2, done : false}
console.log(generator.next()); // { value: 3, done : false}
// yield 표현식이 모두 실행 되었으므로 함수 마지막 까지 실행 됩니다.
// 반환되는 value 값은 undefined, done 프로퍼티는 true 가 됩니다. 
console.log(generator.next()); // { value: undefined, done : true}
```

- 이터레이터의 next 메서드와 달리 제너레이터 객체의 next 메서드 에는 인수를 전달할 수 있다. 
- 제너레이터 객체의 next 메서드에 전달한 인수는 제너레이터 함수의 yield 표현식을 할당받는 변수에 할당 된다.

## 제너레이터의 활용
- 무한 이터러블을 생성하는 제너레이터 함수

```js
// 무한 이터러블을 생성하는 제너레이터 함수
const infiniteFibonacci = (function* () {
  let [pre, cur] = [0, 1];

  while (true) {
    [pre, cur] = [cur, pre + cur];
    yield cur;
  }
}());

// infiniteFibonacci는 무한 이터러블이다.
for (const num of infiniteFibonacci) {
  if (num > 10000) break;
  console.log(num); // 1 2 3 5 8...2584 4181 6765
}
```

- 비동기 처리

```js
const fetch = require('node-fetch'); // node환경에서 fetch 함수 사용하기 위한 라이브러리

// 제너레이터 실행기
const async = generatroFunc => {
	const generator = generatorFunc();
	
	const onResolved = arg => {
		const rst = generator.next(arg);
		
		return rst.done ? rst.value : rst.value.then(res => onResolved(res));
	};
	
	return onResolved; 
};

(async(function* fetchTodo(){
	const url = 'http://jsonplaceholder.typicode.com/todos/1';
	
	const response = yield fetch(url);
	const todo = yield response.json();
	consoloe.log(todo);
})());
```

## async / await
- 제너레이터를 사용해서 비동기 처리를 구현하면 코드가 매우 복잡해짐
- 제너레이터 보다 비동기 처리를 간단하게 구현하기 위해 `async /await` 도입
- 프로미스를 기반으로 동작

```js
const fetch = require('node-fetch');

async function fetchTodo() {
	const url = 'https://jsonplaceholder.typicode.com/todos/1';

	const response = await fetch(url);
	const todo = await response.json();
	console.log(todo);
};
```

### async 함수
- `await` 키워드는 반드시 `async` 함수 내부에서 사용해야 한다.
- `async` 함수는 명시적으로 프로미스를 반환하지 않더라도 암묵적으로 반환값을 `resolve`하는 프로미스를 반환
- 클래스의 `constructor` 메서드는 반드시 값을 반환해야 하므로 `async` 함수가 될 수 없습니다.

### await 키워드
- `await` 키워드는 `settled` 상태(비동기 처리가 수행된 상태)가 될 때까지 대기하다가 settled 상태가 되면 프로미스가 `resolve`한 처리결과를 반환
- `await` 키워드는 반드시 프로미스 앞에서 사용해야 한다.
- 즉, 다음 실행을 일시 중지시켰다가 프로미스가 `settled` 상태가 되면 다시 재개하는 것

### 에러 처리
- 비동기 처리를 위한 콜백 패턴은  try...catch문을 사용해 에러를 캐치 할 수 없다.
- async / await 에서는 try...catch 로 에러 처리 가능
> 에러는 호출자 방향으로 전파 되는데 비동기 함수의 호출자가 해당 비동기 함수가 아니기 때문에
- 물론 async / await은 프로미스로 동작하기 때문에 catch 후속 처리 메서드로도 에러 처리 가능 


