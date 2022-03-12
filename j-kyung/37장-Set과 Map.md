# 37장-Set과 Map
## 37.1 Set
- 중복되지 않는 유일한 값들의 집합
### Set 객체 VS 배열
|                      | 배열 | Set |
|----------------------|:----:|:---:|
| 동일한 값 중복 포함  |   O  |  X  |
| 요소 순서에 의미     |   O  |  X  |
| 인덱스로 요소에 접근 |   O  |  X  |

- 수학적 집합을 구현하기 위한 자료구조
- 교집합, 차집합, 합집합, 여집합 구할 수 있음

### Set 객체의 생성
- Set 생성자 함수로 생성
    - 인수 전달 X => Set 객체 생성
```javascript
const set = new Set();
console.log(set); // set(0) {}
```
- 이터러블을 인수로 전달받아 Set 객체 생성, 이터러블의 중복된 값은 Set 객체에 요소로 저장 X (중복된 요소 제거 가능)

### 요소 개수 확인
- Set.prototype.size
    - setter 함수 X, getter 함수만 존재하는 접근자 프로퍼티
    - 숫자 할당해서 set 객체 요소 개수 변경할 수 없음
``` javascript
const {size} = new Set([1,2,3,3]);
console.log(size); // 3
```

### 요소 추가
- Set.prototype.add
    - 새로 추가된 Set 객체 반환, 연속적으로 호출 가능
    - 중복 요소 추가 허용 X (but, not 에러)
    - NaN은 NaN과 같다고, +0은 -0과 같다고 평가 -> 중복 추가 허용 X
    - 자바스크립트의 모든 값을 요소로 저장 가능

### 요소 존재 여부 확인
- Set.prototype.has
    - 특정 요소의 존재 여부를 나타내는 불리언 값 반환

### 요소 삭제
- Set.prototype.delete
    - 삭제 성공 여부 나타내는 불리언 값 반환
    - 인덱스 X, 삭제하려는 요소값을 인수로 전달
    - 배열과 같이 인덱스를 갖지 않기 때문임 (순서에 의미가 없음)
    - 존재하지 않는 요소 삭제 시 Not 에러, 무시
    - 연속적 호출 불가능

### 요소 일괄 삭제
- Set.prototype.clear
    - 언제나 undefinde 반환

### 요소 순회
- Set.prototype.forEach
    - 콜백 함수와 forEach 메서드의 콜백 함수 내부에서 this로 사용될 객체를 인수로 전달
        - 첫 번째 인수 : 현재 순회 중인 요소 값
        - 두 번째 인수 : 현재 순회 중인 요소 값 (첫 번째와 동일한 값)
        - 세 번째 인수 : 현재 순회 중인 set 객체 자체
    - 순회 순서는 요소가 추가된 순서를 따름

### 집합 연산
- 교집합 : 집합 A와 집합 B의 공통 요소로 구성
```javascript
Set.prototype.intersection = function(set) {
    return new Set([...this].filter(v => set.has(v)));
};
```
- 합집합 : 집합 A와 집합 B의 중복 없는 모든 요소
```javascript
Set.prototype.union = function(set) {
    return new Set([...this, ...set]);
};
```
- 차집합 A-B : 집합 A에는 존재하지만 B에는 존재하지 않는 요소로 구성
```javascript
Set.prototype.difference = functon(set) {
    return new Set([...this].filter(v => !set.has(v)));
};
```
- 부분 집합, 상위 집합 : 집합 A가 집합 B에 포함되는 경우 집합 A는 집합 B의 부분 집합, 집합 B는 집합 A의 상위 집합
```javascript
Set.prototype.inSuperset = function(subset) {
    const supersetArr = [...this];
    return [...subset].every(v => supersetArr.includes(v));
};
```
## 37.2 Map
- 키와 값의 쌍으로 이루어진 컬렉션
### Map VS 객체
|                        |           객체          |         Map         |
|------------------------|:-----------------------:|:-------------------:|
| 키로 사용할 수 있는 값 |     문자열 or 심벌값    | 객체 포함한 모든 값 |
| 이터러블               |            X            |          O          |
| 요소 개수 확인         | Object.keys(obj).length |       map.size      |

### Map 객체의 생성
- Map 생성자 함수로 생성
    - 인수 전달 X => 빈 Map 객체 생성
- 이터러블을 인수로 전달받아 Map 객체 생성
    - 이터러블이 키와 값의 쌍으로 이루어져야 함
- 중복된 키 갖는 요소 존재 -> 값이 덮어써짐 (중복된 키 갖는 요소 존재 x)
```javascript
const map = new Map([['key1','value1'], ['key1','value2']]);
console.log(map); // Map(1) {"key1" => "value2"}
```
### 요소 개수 확인
- Map.prototype.size
- 요소 개수 변경 X

### 요소 추가
- Map.prototype.set
    - 새로운 요소가 추가된 Map 객체 반환
    - 연속적 호출 가능
- 중복된 키 갖는 요소 추가 시 값 덮어써짐 (Not 에러)
- NaN과 NaN, +0과 -0이 같다고 평가
    - 중복 추가 허용 X
- 키 타입 제한 X, 객체 포함한 모든 값을 키로 사용 가능

### 요소 취득
- Map.prototype.get
    - 인수로 키 전달 -> Map 객체에서 인수로 전달한 키를 갖는 값 반환
    - 존재 X -> undefined 반환

### 요소 존재 여부 확인
- Map.prototype.has
    - 특정 요소 존재 여부를 나타내는 불리언 값 반환

### 요소 삭제
- Map.prototype.delete
    - 삭제 성공 나타내는 불리언 값 반환
    - 존재하지 않는 거 삭제 시 에러 X , 무시
    - 연속적 호출 불가능

### 요소 일괄 삭제
- Map.prototype.clear
    - 언제나 undefinde 반환

### 요소 순회
- Map.prototype.forEach
    - 콜백 함수와 forEach 메서드의 콜백 함수 내부에서 this로 사용될 객체를 인수로 전달
        - 첫 번째 인수 : 현재 순회 중인 요소 값
        - 두 번째 인수 : 현재 순회 중인 요소 키
        - 세 번째 인수 : 현재 순회 중인 Map 객체 자체
    - Map 메서드
        - Map.prototype.keys
        - Map.prototype.values
        - Map.prototype.entries
    - 순회 순서는 요소 추가된 순서 따름