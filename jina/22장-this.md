# 22장-this

## 22.1 this 키워드
this 역할:
* ```자신이 속한 객체를 가리키는 식별자를 참조```
* 메서드는 자신이 속한 객체의 프로퍼티를 참조하고 변경할 수 있음
* this바인딩은 함수 호출 방식에 의해 동적으로 결정 
  
```javascript
// 생성자 함수
function Circle(radius){
    this.radius = radius;
}

Circle.prototype.getDiameter = function (){
    return 2 * this.radius;
};

const circle = new Circle(5);

// 객체 리터럴
const circle = {
    radius:5,
    getDiameter(){
        return 2*this.radius
    }
};
```
----

함수 호출되는 방식에 따라 this값이 달라짐
```javascript
function foo(number){
    // 일반함수, window
    console.log(this) // window
}

const Person = {
    name:'Lee',
    getName(){
        // 메서드 내부, 메서드를 호출한 객체
        console.log(this); // {name: "Lee", getName: f} 
        return this.name;
    }
}

function bar(name){
    this.name = name;
    // 생성자 함수, 생성자 함수가 생성할 인스턴스
    console.log(this); // Person {name:"Lee"}
}
```

stric mode에서는 일반 함수 this는 undefined가 바인딩 됨: 일반 함수 내부에서 this 사용할 필요가 없기에

----

## 22.2 함수 호출 방식과 this 바인딩 
this 바인딩은 함수 호출 방식에 따라 동적으로 결정

> 렉시컬 스코프 vs this 바인딩 결정 시기 <br/>
> 렉시컬 스코프는 함수 정의가 평가되어 함수 객체가 생성되는 시점에 상위 스코프 결정 <br/>
> this 바인딩은 함수 호출 시점에 결정

### 22.2.1 일반 함수 호출
* this에 전역 객체 바인딩
* strick mode는 undefined 바인딩
* 일반함수(중첨함수, 콜백 함수 포함)

---

콜백함수에서 this바인딩을 일치시킬 수 있는 방법
1. 콜백 함수 내부에서 this를 할당한 {변수}를 참조
2. 명시적으로 this를 바인딩하는 메서드 사용 .bind(this)
3. 화살표 함수 사용

---

### 22.2.2 메서드 호출

```javascript
const person = {
    name:'Lee',
    getName(){
        // this는 메서드를 호출한 객체에 바인딩
        return this.name;
    }
};

// getName 을 호출한 객체는 person
console.log(person.getName()); // Lee
```
* getName 프로퍼티가 가리키는 함수 객체는 person 객체가 아닌 독립적으로 존재함
* 메서드는 다른 객체의 메서드가 될 수 있고 일반 변수에 할당해 일반 함수로 호출이 가능

```javascript

const anotherPerson = {
    name: 'Kim'
};

anotherPerson.getName = person.getName;

console.log(anotherPerson.getName()); // Kim

const getName = person.getName;

console.log(getName()); //''

```
* 메서드 내부의 this는 **메서드를 호출한 객체에 바인딩**

### 22.2.3 생성자 함수 호출 
생성자 함수가 (미래에) 생성할 인스턴스가 바인딩

```javascript
function Circle(radius){
    this.radius = radius;
    this.getDiameter = function(){
        return 2*this.radius;
    };
}

const circle3 = Circle(5);

console.log(circle3); // undefined
// 일반 함수로 호출된 Circle 내부의 this는 전역 객체를 가리킴
console.log(radius); // 5
```