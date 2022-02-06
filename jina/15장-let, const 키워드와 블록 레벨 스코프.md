# 15장 let, const 키워드와 블록 레벨 스코프

## 15.1 var 키워드로 선언한 변수의 문제점
* ES5까지 변수 선언은 var 키워드만 가능
### 15.1.1 변수 중복 선언 허용
* 먼저 선언된 변수 값이 변경되는 부작용 발생 
```javascript
var x = 1;
var x = 100;
```
### 15.1.2 함수 레벨 스코프
* var 키워드로 선언된 변수는 코드 블록 내에서 선언해도 모두 전역 변수가 됨 

### 15.1.3 변수 호이스팅
```javascript
// 1. 선언 단계
// 2. 초기화 단계
console.log(foo); // undefined

// 3. 할당 단계
foo = 123;

console.log(foo); // 123

// 변수 선언은 런타임 이전에 실행됨
var foo;
```

## 15.2 let 키워드
### 15.2.1 변수 중복 선언 금지
* 같은 스코프 내에서 중복 선언 허용 X
  
### 15.2.2 블록 레벨 스코프
* 블록 레벨 스코프를 따름: 모든 코드 블록 (함수, if문, for문, while 문, try/catch 문 등)

### 15.2.3 변수 호이스팅
* 호이스팅이 발생하지 않는 것처럼 동작
```javascript
console.log(foo); // ReferenceError: foo is not defined
let foo;
```
* **선언 단계, 초기화 단계**가 분리되어 진행
* 선언 단계: 런타임 이전
* 초기화 단계: 변수 선언문에 도달했을 때 실행 
* 할당 단계: 할당문에서 실행 
```javascript
console.log(foo) // ReferencedError: foo is not defined

let foo; // 초기화 단계 실행
```

```javascript
// let 키워드도 호이스팅이 발생함
let foo =1;
{
  //전역 변수 foo가 출력되지 않음 
  console.log(foo); // ReferencedError: Cannot access 'foo' before initializing

  let foo = 2;
  //console.log(foo);
}
```



### 15.2.4 전역 객체와 let
* let, const 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티가 아님

## 15.3 const 키워드
### 15.3.1 선언과 초기화
### 15.3.2 재할당 금지
### 15.3.3 상수
### 15.3.4 const 키워드와 객체

## 15.4 var vs. let vs. const