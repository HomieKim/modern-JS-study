# 33장 7번째 데이터 타입 Symbol
## 1. 심벌이란?
* 변경 불가능한 원시타입
* 이름 충돌 위험이 없는 유일한 프로퍼티 키를 만들기 위해 상용

## 2. 심벌 값의 생성
### Symbol 함수
``` javascript
const mySymbol = Symbol()
```

* 타입으로 변환되지 않음
	* boolean 암묵적으로 타입 변환

### Symbol.for / Symbol.keyFor 
전역 심벌 레지스트리에서 해당 키와 일치하는 심벌 값 검색

## 3. 상수 정의
	* 심벌을 이용해 중복 가능성이 없는 유일무이한 값 사용
	* Enum을 흉내 내기위해 Object.freeze와 Symbol값 사용
```javascript
const Direction = Object.freeze({
	UP:Symbol(‘up’),
	DOWN:Symbol(‘down’),
	LEFT:Symbol(‘left’),
	RIGHT:Symbol(‘right’)
})
```

## 4. 심벌과 프로퍼티 키
	* 심벌 값은 유일무이한 값으로 다른 프로퍼티 키와 *절대 충돌하지 않음*

## 5. 프로퍼티 은닉

## 6. 표준 빌트인 객체 확장
	* 표준 빌트인 객체는 읽기 전용으로 사용하자

## 7. Well-known Symbol
	



