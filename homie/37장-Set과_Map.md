# Set과 Map

## Set
- `Set`객체는 중복되지 않는 유일한 값들의 집합이다.
- `Set`객체는 배열과 유사하지만 다음과 같은 차이가 있다.

| 구분                             |배열  |Set객체| 
|---------------------------------|:---:|:---:|
|동일한 값을 중복하여 포함 할 수 있다 | O | X |
|요소 순서에 의미가 있다.            | O | X |
|인덱스로 요소에 접근할 수 있다.      | O | X |

- `Set`은 수학적 집합을 구현하기 위한 자료구조 입니다, 교집합, 합집합, 차집합, 여집합 등을 구현할 수 있다.

### Set 객체의 생성
- `Set` 생성자 함수에 인수를 전달하지 않으면 빈 Set 객체가 생성 됩니다.
- `Set` 생성자 함수는 이터러블을 인수로 전달받아 Set 객체를 생성한다. (이때, 중복된 값은 Set 객체에 요소로 저장되지 않는다.)
- `Set` 객체의 특성을 활용해서 배열의 중복된 요소를 제거할 수 있습니다.

```js
const set= new Set(); // 빈 set 객체
// 중복 요소는 저장 x
const set2 = new Set([1,2,2,3,4]);
console.log(set2); // Set(4) {1, 2, 3, 4}

// Set을 활용한 배열의 중복 요소 제거
const uniq = (arr) => [... new Set(arr)];
console.log(uniq([2,1,2,3,4,3,4])); // [2,1,3,4]
```


### 요소 개수 확인
- `Set` 객체의 요소 개수를 확인할 때는 `Set.prototype.size` 프로퍼티를 사용합니다.
- size 프로퍼티는 getter함수만  존재하는 접근자 프로퍼티 따라서 size프로퍼티에 숫자를 할당하여 Set객체의 요소 개수를 변경할 수 없다.

```js
const {size} = new Set([1,2,3,3]);
console.log(size); // 3
```

### 요소 추가
- `Set`  객체에 요소를 추가할 때는 `Set.prototype.add` 메서드를 사용한다.
- `add`  메서드는 새로운 요소가 추가된 새로운 `Set` 객체를 반환합니다. 연속적으로 호출이 가능(`method chaining`)

```js
const set = new Set();
set.add(1);
console.log(set); // Set(1) {1}
set.add(2).add(3);
console.log(set); // Set(3) {1, 2, 3}
```
- `Set` 객체는 객체나 배열과 같이 자바스크립트의 모든 값을 요소로 저장할 수 있다.

```js
const set = new Set();

set
  .add(1)
  .add('a')
  .add(true)
  .add(undefined)
  .add(null)
  .add({})
  .add([]);

console.log(set); // Set(7) {1, "a", true, undefined, null, {}, []}
```

### 요소 존재 여부 확인
- `Set` 객체에 특정 요소가 존재하는지 확인 하려면 `Set.prototype.has` 메서드를 사용
- 존재 여부에 따라 불리언 값을 리턴합니다.

```js
const set = new Set([1,2,3]);

console.log(set.has(2)); // true
console.log(set.has(4)); // false
```

### 요소 삭제
- `Set` 객체의 특정 요소를 삭제하려면 `Set.prototype.delete` 메서드를 사용한다.
- 삭제 성공 여부를 나타내는 불리언 값을 반환한다. (method  chaining 불가)
- `delete` 메서드에는 인덱스가 아니라 삭제하려는 요소값을 인수로 전달해야 한다.
(`Set` 객체는 순서에 의미가 없기때문에, 배열과 같이 인덱스 갖지 않는다.)
- 존재 하지 않는 `Set` 객체의 요소를 삭제하면 에러 없이 무시 됨

```js
const set = new Set([1,2,3]);
// 요소 2 삭제
set.delete(2);
console.log(set); // Set(2) {1,3}
// 존재하지 않는 요소 삭제 시 무시 됨
set.delete(4);
console.log(set); // Set(2) {1,3}
```

### 요소 일괄 삭제
 - `Set` 객체의 모든 요소를 일괄 삭제하려면 `Set.prototype.clear` 메서드를 사용한다. (clear 메서드는 항상 undefined 반환)

```js
const set = new Set([1,2,3]);

set.clear();
console.log(set); // Set(0) {}
```

### 요소 순회
- `Set` 객체의 요소를 순회하려면 `Set.prototype.forEach` 메서드를 사용한다.
- `Array.prototype.forEach` 메서드와 유사하게 콜백 함수를 전달받음, 콜백 함수에 3개의 인수를 전달

	1. 첫 번째 인수 : 현재 순회 중인 요소값
	2. 두 번째 인수 : 현재 순회 중인 요소값
	3. 세 번째 인수 : 현재 순회 중인 Set 객체 자체
> 첫 번재 인수와 두 번째 인수가 같은 값을 갖는데, 이는 `Array.prototype.forEach`와 인터페이스를 맞추기 위함

```js
const set = new Set([1, 2, 3]);

set.forEach((v, v2, set) => console.log(v, v2, set));
/*
1 1 Set(3) {1, 2, 3}
2 2 Set(3) {1, 2, 3}
3 3 Set(3) {1, 2, 3}
*/
```

- **Set 객체는 이터러블** 이므로 `for ... of` 문으로 순회할 수 있으며, *스프레드 문법*과 *배열 디스트럭처링*의 대상이 될 수도 있다.

### 집합 연산
- `Set`객체는 수학적 집합을 구현하기 위한 자료구조 이다.

#### 교집합 구현

```js
// 방법1
Set.prototype.intersection = function (set){
	const result = new Set();
	for(const value of set) {
		if(this.has(value)) result.add(value);
	}
	return result;
};

// 방법2
Set.prototype.intersection = function (set) {
	return new Set([...this].filter(v => set.has(v)));
};
```

#### 합집합 구현

```js
// 방법1
Set.prototype.union = function (set) {
	// this(Set 객체)를 복사
	const result = new Set(this);

	for(const value of set) {
		result.add(value);
	}
	return result;
};

// 방법2
Set.prototype.union = function (set) {
	return new Set([...this, ...set]);
};
```

#### 차집합 구현

```js
// 방법1
Set.prototype.difference = function (set) {
	const result = new Set(this);
	for(const value of set) {
		result.delete(value);
	}
	return result;
};

// 방법2
Set.prototype.difference = function (set) {
	return new Set([...this].filter(v => !set.has(v)));
};
```

#### 부분 집합

```js
// 방법1
Set.prototype.isSuperset = function (subset) {
	for(const value of subset) {
		// 모든 요소가 포함 되는지 확인
		if(!this.has(value))return false;
	}
	return true;
};

// 방법2
Set.prototype.isSuperSet = function (subset) {
	const superSet = [...this];
	return [...subset].every(v => superSet.includes(v));
}
```

## Map
- `Map` 객체는 키와 값의 쌍으로 이루어진 컬렉션 이다.
- 객체와 유사하지만 다음과 같은 차이가 있다.

| 구분 | 객체 | `Map` 객체 |
| :--------------------: | :-----------------------: | :-------------------: |
| 키로 사용할 수 있는 값 | `string`, `Symbol` | 객체를 포함한 모든 값 |
| 이터러블 | X | O |
| 요소 개수 확인 | `Object.keys(obj).length` | `map.size` |


### Map 객체의 생성
- `Map` 생성자 함수로 생성
- `Map` 생성자 함수는 이터러블을 인수로 전달받아 Map 객체를 생성, 이때 전달되는 이터러블은 키와 값의 싸응로 이루어진 요소로 구성되어야 한다.
- `Map` 생성자 함수의 인수로 전달한 이터러블에 중복된 키를 갖는 요소가 존자하면 덮어 씌워짐
- 따라서 `Map` 객체에는 중복된 키를 갖는 요소가 존재할 수 없다.

```js
const map1 = new Map([['key1', 'value1'], ['key2', 'value2']]);
console.log(map1); // Map(2) {"key1" => "value1", "key2" => "value2"}

const map2 = new Map([1, 2]); // TypeError: Iterator value 1 is not an entry object

// 중복된 키 불가능
const map = new Map([['key1', 'value1'], ['key1', 'value2']]);
console.log(map); // Map(1) {"key1" => "value2"}
```

### 요소 개수 확인
- `Map.prototype.size`를 사용하여 요소 개수 확인
- `size` 메서드는 getter함수만 존재하는 *접근자 프로퍼티* (즉 사이즈 할당하여 변경 불가)

```js
const { size } = new Map([['key1', 'value1'], ['key2', 'value2']]);
console.log(size); // 2
```

### 요소 추가
- `Map.prtototype.set` 메서드 사용하여 요소를 추가
- set 메서드는 새로운 요소가 추가된 Map 객체를 반환(Method Chaining 가능)

```js
const map = new Map();

map
  .set('key1', 'value1')
  .set('key2', 'value2');

console.log(map); // Map(2) {"key1" => "value1", "key2" => "value2"}
```

- 객체는 문자열 또는 심벌 값만 키로 사용가능하지만 **Map 객체는 키타입에 제한이 없다.** 객체를 포함한 모든 값을 키로 사용할 수 있다.

```js
const map = new Map();

const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

// 객체도 키로 사용할 수 있다.
map
  .set(lee, 'developer')
  .set(kim, 'designer');

console.log(map);
// Map(2) { {name: "Lee"} => "developer", {name: "Kim"} => "designer" }
```


### 요소 취득
- 특정 요소를 취득하려면 `Map.prototype.get` 메서드를 사용한다. 
- `get` 메서드의 인수로  키를 전달하면 전달한 키를 갖는 **값을 반환**
- 키에 맞는 요소가 존재하지 않으면 `undefined` 반환

```js
const map = new Map();

const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

map
  .set(lee, 'developer')
  .set(kim, 'designer');

console.log(map.get(lee)); // developer
console.log(map.get('key')) // undefined
```

### 요소 존재 여부 확인

```js
const lee = { name: "Lee" };
const kim = { name: "Kim" };

const map = new Map([
  [lee, "developer"],
  [kim, "designer"],
]);

map.has(lee); // true
map.has("key"); // false
```

### 요소 삭제
- `Map.prototype.delete` 사용하여 요소 삭제
- `delete` 메서드는 삭제 성공 여부를 불리언 값으로 반환(`Method Chaining` 불가)
- 존재 하지 않는 키로 삭제시 무시 됨

### 요소 일괄 삭제
- `Map.prototype.clear` 메서드 사용
- `clear` 메서드는 언제나 `undefined` 반환

### 요소 순회
- `Map.prototype.forEach` 메서드 사용
- `forEach` 메서드의 인자로 전달되는 콜백함수는 3가지 인수를 전달 받음

	1. 첫 번째 인수 : 현재 순회 중인 요소 값
	2. 두 번째 인수 : 현재 순회 중인 요소 키
	3. 세 번재 인수 : 현재 순회 중인 `Map` 객체 자체

- `Map` 객체는 이터러블 이기 때문에 `for ... of` 순회 가능, 스프레드 문법과 배열 디스트럭처링 할당 가능

```js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

map.forEach((v, k, map) => console.log(v, k, map));
/*
developer {name: "Lee"} Map(2) {
  {name: "Lee"} => "developer",
  {name: "Kim"} => "designer"
}
designer {name: "Kim"} Map(2) {
  {name: "Lee"} => "developer",
  {name: "Kim"} => "designer"
}
*/
```

- Map 객체는 이터러블이면서 동시에 이터레이터인 객체를 반환하는 메서드를 제공

| `Map` 메서드 | 설명 |
| :---------------------: | :-----------------------------------------------------------------------------------------: |
| `Map.prototype.keys` | `Map` 객체에서 요소 키를 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체 반환 |
| `Map.prototype.values` | `Map` 객체에서 요소 값을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체 반환 |
| `Map.prototype.entries` | `Map` 객체에서 요소 키와 요소 값을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체 반환 |

- `Map` 객체는 요소의 순서에 의미를 갖지 않지만 순회하는 순서는 요소가 추가된 순서를 따름 (다른 이터러블 순회와 호환성을 유지하기 위함)
