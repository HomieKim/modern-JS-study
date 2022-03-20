# 39장-DOM
- DOM : HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 `트리 자료구조`

## 39.1 노드
### HTML 요소 & 노드 객체
```javascript
<div class = "greeting"> Hello </div>
//시작 태그, 어트리뷰트 이름, 어트리뷰트 값, 콘텐츠, 종료 태그 순
```
- HTML 요소 : 렌더링 엔진에 의해 파싱 -> DOM을 구성하는 요소 노드 객체로 변환
- 어트리뷰트 -> 어트리뷰트 노드
- 텍스트 콘텐츠 -> 텍스트 노드
- 요소 노드 div, 어트리뷰트 노드 class = "greeting", 텍스트 노드 "Hello"

### 트리 자료구조
- 노드들의 계층 구조로 이루어짐
- 부모 노드와 자식 노드로 구성되어 노드 간의 계층적 구조(부자, 형제 관계) 표현하는 `비선형 자료구조`
- 하나의 최상위 노드에서 시작하며 최상위 노드는 부모 노드가 없음 => 루트 노드라 부름
    - 루트 노드는 0개 이상의 자식 노드를 가지며, 자식 노드가 없는 노드를 리프 노드라고 부름
- DOM을 DOM 트리라고 부르기도 함
- 공백 텍스트 노드 = HTML 요소 사이의 개항 or 공백

### 중요한 노드타입 4가지
- 문서 노드
    - DOM 트리의 최상위에 존재하는 루트 노드, document 객체를 가리킴
    - HTML 문서당 document 객체는 유일함
    - document 객체는 DOM 트리의 노드들에 접근하기 위한 `접근점` 역할
- 요소 노드
    - HTML 요소를 가리키는 객체
    - HTML 요소 간의 중첩에 의해 부자 관계를 가지며, 부자 관계를 통해 정보 구조화 (문서의 `구조`를 표현함을 의미)
    - **부모 노드와 연결되어 있음**
- 어트리뷰트 노드
    - HTML 요소의 어트리뷰트를 가리키는 객체
    - **부모 노드와 연결되지 않음, 요소 노드에만 연결되어 있음**
        - 부모 노드가 없어서 요소 노드의 형제 노드는 아님
        - 어트리뷰트 노드에 접근하여 어트리뷰트를 참조하거나 변경하려면 먼저 요소 노드에 접근해야 함
- 텍스트 노드
    - HTML 요소의 텍스트를 가리키는 객체
        - 문서의 `정보`를 표현함
    - 요소 노드의 자식 노드, 자식 노드를 가질 수 없는 리프 노드임
        - 텍스트 노드는 DOM 트리의 `최종단`
        - 텍스트 노드의 접근하려면 부모 노드의 요소 노드에 접근해야 함

- 노드 객체는 총 12개의 종류가 있음

### 노드 객체의 상속 구조
- DOM을 구성하는 노드 객체는 브라우저 환경에서 추가적으로 제공하는 `호스트 객체`, but, js 객체이므로 프로토타입에 의한 상속 구조 가짐
- 모든 노드 객체는 Object, EventTarget, Node 인터페이스를 상속 받음
- 문서 노드는 Document, HTMLDocument, 요소 노드는 Element, 어트리뷰트 노드는 Attr, 텍스트 노드는 CharacterData 인터페이스를 상속 받음
- 노드 객체의 상속 구조는 개발자 도구의 Elements 패널 우측의 Properties 패널에서 확인 가능
- 모든 노드 객체는 공통적으로 이벤트를 발생시킬 수 있음(EventTarget 인터페이스가 제공), 트리 자료구조의 노드로서 공통적으로 트리 탐색 가능 or 노드 정보 제공 기능 필요 -> Node 인터페이스가 제공
- HTML 요소가 갖는 공통적인 기능은 HTMLElement 인터페이스가 제공
    - but, 필요한 기능을 제공하는 인터페이스가 HTML 요소의 종류에 따라 각각 다름
- 노드 객체는 공통된 기능일수록 프로토타입 체인의 상위에, 개별적인 고유 기능일수록 프로토타입 체인의 하위에 구축하여 노드 객체에 필요한 기능(프로퍼티 or 메서드) 제공하는 상속 구조를 가짐

## 39.2 요소 노드 취득
- HTML 요소를 조작하는 시작점
### id를 이용한 요소 노드 취득
- Document.prototype.getElementById 메서드는 인수로 전달한 id 어트리뷰트 값을 갖는 하나의 요소 노드를 탐색하여 반환
- id 값은 HTML 문서 내에서 유일한 값이어야 함(공백 문자로 구분하여 여러 개의 값을 가질 수 없음)
    - but, 문서 내에 중복된 id 값을 갖는 HTML 요소가 여러 개 존재해도 에러 발생은 안함(여러 개 존재할 가능성은 있음)
        - 이경우 인수로 전달된 id 값을 갖는 첫 번째 요소 노드만 반환
- 만약 id값 갖는 HTML 요소 존재하지 않을 경우 null 반환
- HTML 요소에 id 어트리뷰트 부여 -> id 값과 동일한 이름의 전역 변수가 암묵적으로 선언 -> 해당 노드 객체가 할당
    - but, 이미 선언되어 있으면 재할당되지는 않음

### 태그 이름을 이용한 요소 노드 취득
- Document/Element.prototype.getElementByTagName 메서드는 인수로 전달한 태그 이름을 갖는 모든 요소 노드들을 탐색하여 반환
    - getElementByTagName은 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 HTMLCollection 반환
- 함수는 하나의 값만 반환 가능 -> 여러 개의 값을 반환하려면 배열이나 객체와 같은 자료구조에 담아 반환해야 함
- HTML 문서의 모든 요소 노드를 취득하려면 인수로 `'*'` 전달
    - Document - 메서드 : DOM의 루트 노드인 문서 노드, document를 통해 호출, DOM 전체에서 요소 노드를 탐색하여 반환
    - Element - 메서드 : 특정 요소 노드를 통해 호출 -> 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색하여 반환
- 인수로 전달된 태그 이름을 갖는 요소가 존재하지 않으면 빈 HTMLCollection 객체 반환

### class를 이용한 요소 노드 취득
- Document/Element.prototype.getElementByClassName 메서드는 인수로 전달한 class 어트리뷰트 값을 갖는 모든 요소 노드들을 탐색, 반환
    - 공백으로 구분하여 여러 개의 class 지정 가능
- 인수로 전달된 class 값을 갖는 요소가 존재하지 않으면 빈 HTMLCollection 객체 반환

### CSS 선택자를 이용한 요소 노드 취득
- 스타일을 적용하고자 하는 HTML 요소를 특정할 때 사용
```javascript
* { ... } // 전체 선택자 : 모든 요소를 선택
p { ... } // 태그 선택자 : 모든 p 태그 요소를 모두 선택
#foo { ... } // id 선택자 : id 값이 'foo'인 요소 모두 선택
.foo { ... } // class 선택자 : class 값이 'foo'인 요소 모두 선택
input[type=text] { ... } // 어트리뷰트 선택자 : input 요소 중 type 어트리뷰트 값이 'text'인 요소 모두 선택
div p { ... } // 후손 선택자 : div 요소의 후손 요소 중 p 요소 모두 선택
div > p { ... } // 자식 선택자 : div 요소의 자식 요소 중 p 요소 모두 선택
p + ul { ... } // 인접 형제 선택자 : p 요소의 형제 요소 중에 p 요소 바로 뒤에 위치하는 ul 요소 선택
p - ul { ... } // 일반 형제 선택자 : p 요소의 형제 요소 중에 p 요소뒤에 위치하는 ul 요소 모두 선택
a:hover { ... } // 가상 클래스 선택자 : hover 상태인 a 요소 모두 선택
p::before { ... } // 가상 요소 선택자 : p 요소의 콘텐츠의 앞에 위치하는 공간 선택, 일반적으로 context 프로퍼티와 함께 사용
```

- Document/Element.prototype.querySelector 메서드
    - 인수로 전달한 요소 노드가 여러 개인 경우 첫 번째 요소 노드만 반환
    - 인수로 전달된 요소 노드가 존재하지 않을 경우 null 반환
    - 인수로 전달한 CSS 선택자가 문법에 맞지 않을 시 DOMException 에러 발생
- Document/Element.prototype.querySelectorAll 메서드
    - 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 NodeList 객체 반환
    - 인수로 전달된 요소 존재하지 않을 경우 빈 NodeList 객체 반환
    - 문법에 맞지 않을 시 DOMException 에러 발생
    - HTML 문서의 모든 요소 노드 취득 -> 인수로 `'*'` 전달

### 특정 요소 노드를 취득할 수 있는지 확인
- Element.prototype.matches 메서드
    - 인수로 전달한 CSS 선택자를 통해 특정 요소 노드를 취득할 수 있는지 확인
    - 이벤트 위임 사용 시 유용

### HTMLCollection과 NodeList
- DOM API가 여러 개의 결과값을 반환하기 위한 DOM 컬렉션 객체
- 유사 배열 객체 이면서 이러터블 => for...of 로 순회 가능, 스프레드 문법 사용 가능
- **상태 변화를 실시간으로 반영하는 `살아 있는 객체`**
    - 언제나 live 객체로 동작
    - but, NodeList는 대부분의 경우 정적 상태 유지하는 non-live 객체로 동작, 경우에 따라 live 객체로 동작
- HTMLCollection
    - 실시간으로 노드 객체의 상태 변경을 반영하여 요소 제거 가능
    - for문으로 순회하면서 상태 변경시 주의
        - 해결 방법 : 역방향 순회 or while문 무한 반복
- NodeList
    - NodeList.prototype은 forEact, item, entries, keys, values 메서드 제공
    - `childNodes 프로퍼티가 반환`하는 NodeList 객체는 실시간으로 노드 객체의 상태 변경을 반영하는 live 객체로 동작

## 39.3 노드 탐색
- Node, Element 인터페이스는 트리 탐색 프로퍼티 제공
- Node.prototype : parentNode, previousSibling, firstChild, childNodes 프로퍼티 제공
- Element.prototype : previousElementSibling, nextElementSibling, chlidren 프로퍼티 제공
- 노드 탐색 프로퍼티는 모두 접근자 프로퍼티
- setter 없이 getter만 존재 -> 참조만 가능한 읽기 전용 접근자 프로퍼티
    - 값 할당 시 아무 에러없이 무시함

### 공백 텍스트 노드
- HTML 문서의 공백 문자는 공백 텍스트 노드 생성

### 자식 노드 탐색
- Node.prototype.childNodes 프로퍼티
    - 자식 노드 모두 탐색 -> NodeList에 담아 반환
        - **텍스트 노드도 포함되어 있을 수 있음**
- Element.prototype.children 프로퍼티
    - 자식 노드 중 요소 노드만 모두 탐색 -> HTMLCollection에 담아 반환
        - **텍스트 노드 포함하지 않음**
- Node.prototype.firstChild - 첫 번째 자식 노드 반환
- Node.prototype.lastChild - 마지막 자식 노드 반환
- Element.prototype.firstElementChild - 첫 번째 자식 요소 노드 반환
- Element.prototype.lastElementChild - 마지막 자식 요소 노드 반환

### 자식 노드 존재 확인
- Node.prototype.hasChildNodes 메서드
    - 자식 노드 존재 : ture 반환
    - 자식 노드 존재 X : false 반환
    - 반환 시 텍스트 노드도 포함
        - 텍스트 노드 아닌 요소 모든 존재 확인 -> Children.length or Element 인터페이스의 childElementCount 프로퍼티 사용

### 요소 노드의 텍스트 노드 검색
- firstChild 프로퍼티 접근

### 부모 노드 탐색
- Node.prototype.parentNode 프로퍼티 사용
- 텍스트 노드는 DOM 트리의 최종단 노드인 리프 노드라서 부모 노드가 텍스트 노드 인 경우는 없음

### 형제 노드 탐색
- 텍스트 노드 or 요소 노드만 반환
- Node.prototype.previousSibling : 부모가 같은 형제 노드 중 자신의 이전 형제 노드 탐색, 반환 (텍스트 노드 반환 가능)
- Node.prototype.nextSibling : 부모가 같은 형제 노드 중 자신의 다음 형제 노드 탐색, 반환 (텍스트 노드 반환 가능)
- Element.prototype.previousSibling : 부모가 같은 형제 노드 중 자신의 이전 형제 노드 탐색, 반환 (텍스트 노드 반환 불가능)
- Element.prototype.nextSibling : 부모가 같은 형제 노드 중 자신의 다음 형제 노드 탐색, 반환 (텍스트 노드 반환 불가능)

## 39.4 노드 정밀 취득
- Node.prototype.nodeType : 노드 객체 종류(노드 타입)를 나타내는 상수 반환
    - Node.ELEMENT_NODE : 요소 노드 타입을 나타내는 상수 1 반환
    - Node.TEXT_NODE : 텍스트 노드 타입을 나타내는 상수 3 반환
    - Node.DOCUMENT_NODE : 문서 노드 타입을 나타내는 상수 9 반환
- Node.prototype.nodeName : 노드 이름을 문자열로 반환
    - 요소 노드 : 대문자 문자열로 태그 이름 반환"
    - 텍스트 노드 : 문자열인 "#text" 반환
    - 문서 노드 : 문자열인 "#document" 반환

## 39.5 요소 노드의 텍스트 조작
### nodeValue
- setter/getter 모두 존재하는 접근자 프로퍼티
    - 참조와 할당 모두 가능
- 노드 객체의 nodeValue 프로퍼티 참조 -> 노드 객체 값 반환
    - 텍스트 노드 아닌 노드 nodeValue 프로퍼티 참조 -> null을 반환
- 텍스트 노드의 nodeValue 프로퍼티에 값 할당
    - 텍스트 변경 가능
    1. 텍스트 변경할 요소 노드 취득 -> 취득한 요소 노드의 텍스트 노드 탐색(firstChild 프로퍼티를 사용하여 탐색)
    2. 탐색한 텍스트 노드의 nodeValue 프로퍼티 사용하여 텍스트 노드 값 변경

### textContent
- setter/getter 모두 존재하는 접근자 프로퍼티
    - 요소 노드 텍스트 + 모든 자손 노드 텍스트 모두 취득/변경
- 요소 노드의 childNodes 프로퍼티가 반환한 모든 노드들의 텍스트 모두 반환 (HTML 마크업은 무시)
- 요소 노드의 textContent 프로퍼티에 문자열 할당 -> 요소 노드의 모든 자식 노드가 제거 -> 할당한 문자열이 텍스트로 추가 (HTML 마크업 파싱 X)

## 39.6 DOM 조작
- 새로운 노드를 생성하여 DOM에 추가 or 기존 노드 삭제/교체
- DOM에 새로운 노드가 추가/삭제되면 리플로우와 리페인트 발생

### innerHTML
- setter/getter 모두 존재하는 접근자 프로퍼티
- 요소 노드의 HTML 마크업 취득/변경
- HTML 마크업이 포함된 문자열 그대로 반환
- 요소 노드의 innerHTML 프로퍼티에 문자열 할당 -> 요소 노드의 모든 자식 노드가 제거, 할당한 문자열에 포함되어 있는 HTML 마크업이 파싱 -> 요소 노드의 자식 노드로 DOM에 반영

### insertAdjacentHTML 메서드
- 기존 요소 제거 X, 위치 지정해 새로운 요소 삽입
- 두 번째 인수로 전달한 HTML 마크업 문자열 파싱, 그 결과로 생성된 노드를 첫 번째 인수로 전달한 위치에 삽입하여 DOM에 반영
- 첫 번째 인수로 전달할 수 있는 문자열은 beforebegin, afterbegin, beforeend, afterend 4가지