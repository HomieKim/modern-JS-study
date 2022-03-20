# 39장-DOM
DOM
* HTML 문서의 계층적 구조와 정보를 표현
* DOM API를 제공해 제어가 가능
	* 이 API에는 노드 타입에 따라 필요한 기능을 제공

## 1. 노드
### HTML 요소와 노드 객체
* HTML 요소: HTML 문서를 구성하는 개별적인 요소
* 노드 객체: HTML 요소는 렌더링 엔진에 의해 노드 객체로 변환
```html
<div class=“greeting”>Hello</div>
요소 노드: div
어트리뷰트 노드: class=“greeting”
텍스트 노드: Hello
```	

### 노드 객체의 타입
* 문서 노드: 최상위 루트 노드, document 객체
* 요소 노드: HTML 요소 객체
* 어트리뷰트 노드: HTML 요소의 어트리뷰트
* 텍스트 노드: HTML요소 노드의 자식 노드

### 노드 객체의 상속 구조
* 노드 객체도 자바스크립트 객체이므로 프로토타입에 의한 상속 구조를 가짐
-> 모든 노드 객체가 공통으로 갖는 기능, 고유 기능이 분리 됨
* HTMLElement 인터페이스: HTML요소가 갖는 공통적인 기능

## 2. 요소 노드 취득
### id를 이용한 요소 노드 취득 
```javascript
const $elem = document.getElementById(‘banana’)
// 부수효과
console.log(foo === document.getElementById(‘foo’)
```

### 태그이름을 이용한 요소 노드 취득
```javascript
// document를 통해 DOM 전체에서 요소 노드 탐색
const $elems = document.getElementsByTagName(‘li’)

// #fruits 자손 노드에서 탐색
const $fruits = document.getElementById(‘fruits’)
const $listFromFruits = $fruits.getElementsByTagName(‘li’)
```

### class를 이용한 요소 노드 취득
```javascript
const $elems = document.getElementsByClassName(‘fruit’)
```

### CSS 선택자를 이용한 요소 노드 취득
*하나의 요소 노드를 탐색해 반환* 
```javascript
const $elem = document.querySelector(‘.banana’)
const $elems = document.querySelectorAll(‘ul > li’)
```
* getElementById, getElementsBy*** 메서드보다 다소 느림
* CSS선택자 문법으로 구체적인 조건으로 요소를 취득할 수 있음

### HTMLCollection과 NodeList
* 노드상태 변화를 실시간으로 반영하는 살아 있는 객체
* 유사 배열 객체
* 이터러블

HTMLCollection
* 항상 살아있는 객체
* 부수효과가 있으므로 역방향 순회, while, 고차함수를 이용

NodeList
* querySelectorAll()은 NodeList 객체를 반환
* 이때 NodeList는 non-live
	* childNodes 프로퍼티가 반환하는 NodeList는 live객체

-> DOM컬렉션을 안전하게 사용하려면 HTMLCollection 또는 NodeList 객체를 배열로 변환해 사용을 권장


## 3. 노드 탐색
### 공백 텍스트 노드

### 자식 노드 탐색
| 프로퍼티                   | 설명                           |
| -------------------------- | ------------------------------ |
| Node.prototype.childNodes  | 요소 노드또는 텍스트 노드 포함 |
| Element.prototype.children | 요소 노드만                    |

### 자식 노드 존재 확인

```html
<ul id=“fruits”>
</ul>
```
```javascript
const $fruits = document.getElementById(‘fruits’)

// 텍스트 노드 포함
console.log($fruits.hasChildNodes()); // true

// 요소 노드만
console.log($fruits.children.length); // 0
console.log($fruits.childElementCount); // 0
```

### 요소 노드의 텍스트 노드 탐색 
```html
<div id=“foo”>Hello</div>
```
```javascript
console.log(document.getElementById(‘foo’).firstChild); //#text
```

### 부모 노드 탐색
### 형제 노드 탐색

## 4. 노드 정보 취득

## 5. 요소 노드의 텍스트 조작
### nodeValue
### textContent
```html
<div id=“foo”>Hello <span>world!</span></div>
<script>
	const $foo = document.getElementById(‘foo’)
console.log($foo.nodeValue); //null
console.log($foo.firstChild.nodeValue); // Hello
console.log($foo.lastChild.nodeValue); // world!

console.log($foo.textContent); // Hello world!
</script>
```
* InnerText는 사용 권고 X
	* CSS에 순종적
	* textContent보다 느림

### innerHTML
```html
<div id=“foo”>Hello <span>world!</span></div>
<script>
console.log(document.getElementById(‘foo’).innerHTML); // Hello<span>world!</span>
</script>
```
innerHTML로 DOM조작 가능하지만 XSS에 취약

### insertAdjacentHTML
기존 요소를 제거하지 않고 위치를 지정해 새로운 요소 삽입

### 노드 생성과 추가
* createElement()
* appendChild()

### 복수의 노드 생성과 추가
* 컨테이너 요소를 생성 - 불필요한 컨테이너 요소 추가
* DocumentFragment 노드 사용 - 불필요한 컨테이너 요소 추가 X

### 노드삽입
* appendChild() - 마지막 노드로 추가
* insertBefore() - 지정한 위치에 노드 삽입

### 노드 이동

### 노드 복사
* Node.prototype.cloneNode([deep: false | true])

### 노드 교체 
* Node.prototype.replaceChild(newChild, oldChild)

### 노드 삭제
* RemoveChild(child)

## 7. 어트리뷰트
### 어트리뷰트 노드와 attributes 프로퍼티
* HTML 공통 어트리뷰트: 글로벌 어트리뷰트, 이벤트 헨들러 어트리뷰트
* 한정적 HTML 요소: type, value, checked 어트리뷰트
* 요소 노드의 attributes 프로퍼티는 어트리뷰트 노드의 참조가 저장

### HTML 어트리뷰트 조작

### HTML 어트리뷰트 vs DOM 프로퍼티
 HTML 어트리뷰트
* HTML 요소의 초기 상태 지정
* 첫 렌더링시 어트리뷰트 값
* getAttribute(‘value’)

DOM프로퍼티
* 사용자 입력에 따른 최신 상태
* .value

HTML 어트리뷰트와 DOM프로퍼티의 대응 관계
* id attribute === id property


### data 어트리뷰트와 dataset 프로퍼티
* HTMLElement.dataset 프로퍼티로 취득 가능 

## 8. 스타일
### 인라인 스타일 조작
* HTMLElement.prototype.style

### 클래스 조작
### 요소에 적응되어 있는 CSS 스타일 참조
* GetComputedStyle
* 모든 스타일이 조합되어 최종적으로 적용된 스타일

## 9. DOM 표준 