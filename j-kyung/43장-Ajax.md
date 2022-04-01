# 43장-Ajax
## 43.1 Ajax란?
- js를 사용하여 브라우저가 서버에게 비동기 방식으로 데이터를 요청, 서버가 응답한 데이터를 수신하여 웹페이지를 동적으로 갱신하는 프로그래밍 방식
    - 서버로부터 웹페이지의 변경에 필요한 데이터만 비동기 방식으로 전송받아 웹페이지를 변경할 필요가 없는 부분은 다시 렌더링 X, 변경할 필요가 있는 부분만 한정적으로 렌더링하는 방식
        - 빠른 퍼포먼스와 부드러운 화면 전환 가능

### 장점 3가지
1. 필요한 데이터만 전송받음
    - 불필요한 데이터 통신 발생 X
2. 변경 필요 X -> 리렌더링 X
    - 화면 깜박임 현상 X
3. 비동기 방식 동작
    - 서버에게 요청 보낸 후 블로킹 발생 X

## 43.2 JSON
- 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷

### JSON 표기 방식
- 키와 값으로 구성된 순수한 텍스트
- 반드시 `큰따옴표`로 묶어야 함(값은 상관 없지만, 문자열은 반드시 큰따옴표로 묶기)

### JSON.stringfy
- 객체를 JSON 포맷의 문자열로 변환(배열도 가능) -> `직렬화`

### JSON.parse
- JSON 포맷의 문자열을 객체로 변환(배열도 가능) -> `역직렬화`

## 43.3 XMLHttpRequest
- HTTP 요청 전송과 HTTP 응답 수신을 위한 다양한 메서드와 프로퍼티 제공

### 객체 생성
- XMLHttpRequest 생성자 함수 호출하여 생성(브라우저 환경에서만 정상적으로 실행)

### 객체의 프로퍼티와 메서드
- 프로토타입 프로퍼티
    - readyState : 현재 상태를 나타내는 정수
    - status : 요청에 대한 응답 상태를 나타내는 정수
    - statusText : 응답 메시지 나타내는 문자열
    - responseText : HTTP 응답 타입
    - response : 응답 몸체
    - responseText : 응답 문자열
- 이벤트 핸들러 프로퍼티
    - onreadystate : readystate 값 변경된 경우
    - onloadstart : 응답 받기 시작한 경우
    - onprogress : 응답을 받는 도중 주기적으로 발생
    - onabort : abort 메서드에 의해 중단된 경우
    - onerror : 에러 발생 경우
    - onload : 성공적으로 완료한 경우
    - ontimeout : 시간 초과 경우
    - onloadend : 완료한 경우 (성공 or 실패)
- 메서드
    - open : 초기화
    - send : 전송
    - abort : 중단
    - setRequestHeader : 특정 HTTP 요청 헤더의 값 설정
    - getResponseHeaddr : 특정 HTTP 요청 헤더의 값 문자열로 변환
- 정적 프로퍼티
    - UNSENT - 0 - open 호출 이전
    - OPENED - 1 - open 호출 이후
    - HEADERS_RECEIVED - 2 - send 호출 이후
    - LOADING - 3 - 서버 응답 중
    - DONE - 4 - 서버 응답 완료

### HTTP 요청 전송
1. XHMLHttpRequest.prototype.open 메서드로 HTTP 요청 초기화
2. 필요에 따라 XHMLHttpRequest.prototype.setRequestHeader 메서드로 특정 HTTP 요청의 헤더 값 실행
    - 반드시 open 호출 이후에 호출해야 함
3. XMLHttpRequest.prototype.send 메서드로 HTTP 요청 전송
    - GET : 데이터를 URL의 일부분인 쿼리 문자열로 서버에 전송
        - send 메서드에 페이로드로 전달한 인수는 무시, 요청 몸체는 null로 설정
    - POST : 데이터를 요청 몸체에 담아 전송