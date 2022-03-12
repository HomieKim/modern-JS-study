# 37장 Set과 Map
## 1. Set
중복되지 않는 유일한 값들의 집합

### Set 객체의 생성
* Set 생성자 함수
* 중복된 요소를 제거

### 요소 개수 확인 - Set.prototype.size

### 요소추가 - Set.prototype.add

### 요소 존재 여부 확인 - Set.prototype.has

### 요소삭제 - Set.prototype.delete

### 요소일괄 삭제 - Set.prototype.clear

### 요소 순회 - Set.prototype.forEach
* Set 객체는 이터러블
* for…of, 스프레드, 배열 디스트럭처링의 대상

### 집합연산
* 교집합: intersection()
* 합집합: union(), 배열 디스트럭처링
* 차집합: difference
* 부분 집합과 상위 집함: isSuperSet

## Map
* 키와 값의 쌍으로 이루어진 컬렉션
* Map은 *이터러블*
* 모든 키는 유니크
* 키 타입에 제한이 없음 (객체의 키값은 문자열 또는 심벌 값을 가짐)