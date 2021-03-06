# 20장-strict mode
## 20.1 strict mode란?
- 자바스크립트 언어의 문법을 좀 더 엄격히 적용
    - 오류 발생시킬 가능성 높거나 js 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 명시적 에러 발생시킴
### 린트도구
- 정적 분석 기능을 통해 소스코드 실행 이전에 소스코드 스캔
    - 문법적 오류 + 잠재적 오류 찾아내고 오류 원인 리포팅해주는 도구
- strict mode가 제한하는 오류 + 코딩 컨벤션을 설정 파일 형태로 정의, 강제

## 20.2 strict mode의 적용
- 전역의 선두 or 함수 몸체의 선두에 `'use strict';` 추가
    - 전역에 선두 : 스크립트 전체에 적용
    - 함수 몸체에 선두 : 해당 함수, 중첩 함수에 적용

## 20.3 전역에 strict mode 적용은 피하자
- 전역에 적용하면 스크립트 단위로 적용됨
    - 다른 스크립트에 영향 X, 해당 스크립트에 한정 적용

## 20.4 함수 단위로 strict mode 적용도 피하자
- strict mode는 즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 바람직

## 20.5 strict mode가 발생시키는 에러
### 암묵적 전역
- 선언 X, 변수 참조 -> ReferenceError 발생
### 변수, 함수, 매개변수의 삭제
- delete 연산자로 변수, 함수, 매개변수 삭제 -> SyntaxError 발생
### 매개변수 이름의 중복
- 중복된 매개변수 이름 사용 -> SyntaxError 발생
### with문 사용
- SyntaxError 발생

## 20.6 strict mode 적용에 의한 변화
### 일반 함수의 this
- 일반 함수로 호출 시 this에 undefined 바인딩 (Not 에러)
### arguments 객체
- 매개변수에 전달된 인수를 재할당하여 변경 -> arguments 객체에 반영 X