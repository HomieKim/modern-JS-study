# Ajax
## Ajax란?

- Ajax란 `Asynchronous JavaScript And Xml` 
- 자바스크립트를 사용하여 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신하여 웹페이지를 동적으로 갱신하는 프로그래밍 방식
- 브라우저에서 제공하는 Web API인 `XMLHttpRequest` 객체를 기반으로 동작
- 이전의 웹페이지는 `HTML`을 서버로부터 전송 받아 웹페이지 전체를 처음부터 다시 렌더링 하는 방식으로 동작 함

#### 전통적인 방식의 단점
1. 이전 웹페이지와 차이가 없어 변경할 필요 없는 부분까지 포함한 완전한 HTML을 서버로 부터 매번 다시 전송 받기 때문에 불필요한 데이터 통신이 발생
2. 화면 전환이 일어나면서 순간적으로 깜빡이는 현상 발생
3. 클라이언트와 서버와의 통신이 동기방식으로 동작하기 때문에 서버로 부터 응답이 있을 때까지 다음 처리는 블로킹 된다.

- 즉, Ajax를 사용해 서버로 부터 웹페이지의 변경에 필요한 데이터만 비동기 방식으로 전송 받아 필요한 부분만 한정적으로 렌더링 하는 방식을 사용

#### Ajax 방식의 장점
1. 변경할 부분을 갱신하는데 필요한 데이터만 서버로 부터 전송 받기 때문에 불필요한 데이터 통신이 발생하지 않는다.
2.  변경할 필요가 없는 부분은 다시 렌더링 하지 않는다. (화면이 깜빡이는 현상 발생하지 않음)
3. 클라이언트와 서버와의 통신이 비동기 방식으로 동작하기 때문에 서버에게 요청을 보낸 후 블로킹 발생하지 않음

## JSON
- JSON(`JavaScript Object Notation`)은 클라이언트와 서버간의 HTTP 통신을 위한 텍스트 데이터 포맷

###  JSON 표기 방식
- 자바스크립트의 객체 리터럴과 유사하게 키와 값으로 구성된 순수한 텍스트이다.

```js
{
  "name": "Lee",
  "age": 20,
  "alive": true,
  "hobby": ["traveling", "tennis"]
}
```

- JSON의 키는 반드시 큰따옴표로 묶어야 한다. 
- 값은 객체 리터럴과 같은  표기법을 그대로 사용할 수 있따.
- 단, 문자열은 반드시 큰 따옴표로 묶어야 한다.

### JSON.stringify
- `JSON.stringify` 메서드는 객체를 JSON 포맷의 문자열로 변환
- 클라이언트가 서버로 객체를 전송하려면 객체를 문자열화 해야하는데 이를 직렬화(`serializing`)라고 한다.
- `JSON.stringify` 메서드는 객체뿐만 아니라 배열도 JSON 포맷의 문자열로 변환

### JSON.parse
- `JSON.parse`메서드는 JSON 포맷의 문자열을 객체로 변환
- 서버로 부터 받아온 JSON 데이터는 문자열
- 이 문자열을 객체로서 사용하기 위해 객체화 하는 것을 역직렬화(`deserializing`) 이라고 한다.
- 배열 변환된 JSON 포맷의 경우, 배열 객체로 변환하며, 배열의 요소가 객체인 경우 배열의 요소 가지 객체로 변환한다.

## XMLHttpRequest
- 브라우저는 주소창이나 `form` 태그 또는 `a`태그를 통해 HTTP요청 기능을 제공합니다.
- 자바스크립트를 사용하여 HTTP 요청을 전송하려면 `XMLHttpRequest`객체를 사용해야 합니다.
- `XMLHttpRequest` 객체는 생성자 함수를 통해 생성하고, 브라우저에서 제공하는 Web API이므로 브라우저 환경에서만 동작 합니다. 

```js
const xhr = new XMLHttpRequest();
```

### XMLHttpRequest 객체의 프로퍼티와 메서드
- `XMLHttpRequest`는 다양한 프로퍼티와 메서드를 제공합니다.
> 중요 프로퍼티와 메서드만 정리

### 프로토타입 프로퍼티
- readyState : HTTP 요청의 현재 상태를 나태내는 정수
- status : HTTP 요청에 대한 응답 상태를 나타내는 정수
- statusText : HTTP 요청에 대한 응답 메시지를 나타내는 문자열
- responseType :  HTTP 응답 타입 (`document`, `json`, `blob`, `arraybuffer`...)
- response : HTTP 요청에 대한 응답 몸체

#### 이벤트 핸들러 프로퍼티
- onreadystatechange : readyState 값이 변경된 경우
- onerror : HTTP 요청에 에러가 발생한 경우
- onload : HTTP 요청이 성공적으로 완료된 경우

#### 메서드
- open : HTTP 요청 초기화
- send : HTTP  요청 전송
- abort : 이미 전송된 HTTP 요청 중단
- setRequestHeader : 특정 HTTP 요청 헤더의 값을 설정


### HTTP 요청 전송
- 다음 순서를 따름
1.  `XMLHttpRequest.prototype.open`  메서드로  `HTTP`  요청을 초기화
2.  필요에 따라  `XMLHttpRequest.prototype.setRequestHeader`  메서드로 특정  `HTTP`  요청의 헤더 값을 설정
3.  `XMLHttpRequest.prototype.send`  메서드로  `HTTP`  요청 전송

```js
const xhr = new XMLHttpRequest();
xhr.open('GET', '/user');
xhr.setRequestHeader('content-type', 'application/json');
xhr.send();
```

#### open 메서드
- open 메서드는 서버에 전송할 HTTP요청을 초기화 합니다.

```js
xhr.open(method, url[, async])
```

- method : HTTP 요청 메소드(`GET`, `POST`, `PUT`, `DELETE` ... )
- url : 요청을 전송할 URL
- async : 비동기 요청 여부, 옵션으로 기본값은 true

#### send 메서드
- open으로 초기화된 요청을 서버에 전송
> GET  요청 메서드의 경우 URL의 일부분인 쿼리 문자열(`query string`)으로 서버에 전송 
> POST 요청의 경우 데이터를 요청 몸체(`request body`)에 담아 전송

- send 메서드는 요청 몸체에 담아 전송할 데이터를 인수로 전달할 수 있다. 
- 이때 , `JSON.stringify` 메서드를 사용해 직렬화 한 다음 전송

#### setRequestHeader
- HTTP 요청의 헤더 값을 설정

### 응답 처리
- 서버에서 받아온 응답을 처리하려면 `XMLHttpRequest` 객체가 발생 시키는 이벤트를 캐치 해야한다. 
- 이벤트 핸들러 프로퍼티를 사용해 `readyState`가 변경될 때마다 이벤트를 캐치 할 수 있다.

```js
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
// https://jsonplaceholder.typicode.com은 Fake REST API를 제공하는 서비스다.
xhr.open('GET', 'https://jsonplaceholder.typicode.com/todos/1');

xhr.send();

//  HTTP 요청의 현재 상태를 나타내는 readyState 프로퍼티가 변경될 때 이벤트 발생
xhr.onreadystatechange = () => {
  // 만약 서버 응답이 아직 완료되지 않았다면 아무런 처리를 하지 않는다.
  if (xhr.readyState !== XMLHttpRequest.DONE) return;

  if (xhr.status === 200) {
    console.log(JSON.parse(xhr.response));
    // {userId: 1, id: 1, title: "delectus aut autem", completed: false}
  } else {
    console.error('Error', xhr.status, xhr.statusText);
  }
};
```

- `readystatechange` 대신 `load` 이벤트를 캐치 해도 된다.
- `load` 이벤트는 요청이 성공적으로 완료된 경우 발생

```js
const xhr = new XMLHttpRequest();

xhr.open('GET', 'https://jsonplaceholder.typicode.com/todos/1');

xhr.send();

// load 이벤트는 HTTP 요청이 성공적으로 완료된 경우 발생한다.
xhr.onload = () => {
  if (xhr.status === 200) {
    console.log(JSON.parse(xhr.response));
    // {userId: 1, id: 1, title: "delectus aut autem", completed: false}
  } else {
    console.error('Error', xhr.status, xhr.statusText);
  }
};
```
