# 클로저

클로저는 함수와 그 함수가 선언된 렉시컬 환경의 조합이다.

중첩함수가 아니라면 A함수를 B함수 내부에서 호출한다 하더라도 A함수는 B함수의 변수에 접근할 수 없다.

그 이유는 자바스트립트는 렉시컬 스코프를 따르는 프로그래밍 언어이기 때문이다.

## 1\. 렉시컬 스코프

- 스코프의 실체는 실행 컨텍스트의 렉시컬 환경이다.
- 스코프 체인은 "외부 렉시컬 환경에 대한 참조"를 통해 상위 렉시컬 환경과 연결되는 것이다.

<u>렉시컬 환경의 "외부 렉시컬 환경에 대한 참조"에 저장할 참조값, 즉 상위 스코프에 대한 참조는 함수 정의가 평가되는 시점에 함수가 정의된 위치에 의해 결정한다.</u>

## 2\. 함수 객체의 내부 슬롯

### 함수 객체의 내부슬롯 \[\[Environment\]\]에 저장

- 현재 실행중인 실행 컨텍스트의 렉시컬 환경의 참조(상위 스코프)를 저장한다.
- 자신이 정의된 환경(상위 스코프)의 참조를 저장한다.

### 함수 코드 평가 순서

1\. 함수 실행 컨텍스트 생성

2\. 함수 렉시컬 환경 생성

2.1 함수 환경 레코드 생성

2.2 this 바인딩

2.3 외부 렉시컬 환경에 대한 참조 결정

<u>외부 렉시컬 환경에 대한 참조에는 함수 객체의 내부 슬롯에 저장된 렉시컬 환경의 참조가 할당.</u>

## 3\. 클로저와 렉시컬 환경

**클로저** : <u>외부함수보다 중첩 함수가 더 오래 유지되는 경우에 생명 주기가 종료한 외부 함수의 변수를 참조하는 중첩함수.</u>

**자유 변수** : <u>클로저에 의해 참조되는 상위 스코프의 변수</u>

### 클로저의 특징

- 호출과 상관없이 함수는 상위 스코프의 식별자를 참조, 변경할 수 있어야 한다.
- 외부함수는 실행 컨텍스트 스택에서 제거되지만 렉시컬 환경까지 소멸하지 않는다.
- 중첩함수가 상위 스코프의 식별자를 참조하고 외부함수보다 더 오래 유지되어야 한다.
- 클로저가 참조하고 있는 식별자만 기억한다.

## 4\. 클로저의 활용

클로저는 상태를 안전하게 변경하고 특정 함수에게만 상태 변경을 허용하여 안전하게 변경하고 유지하기 위해 사용

```
//카운트 상태 증가
const increase = (function() {
 let num = 0;
  //클로저
  return function() {
   return ++num;
  };
}());

console.log(increase()); //1
console.log(increase()); //2
console.log(increase()); //3
```

- increase 에 할당된 함수는 상위 스코프인 즉시 실행 함수의 렉시컬 환경을 기억하는 클로저이다.
- 즉시 실행 함수는 한번만 실행되므로 increase가 호출될 때마다 num이 초기화 되지 않는다.

```
//카운트 상태 증가, 감소
const counter = (function () {
 let num = 0;
  return {
   increase() {
    return ++num;
   },
   decrease() {
    return num > 0 ? --num : 0;
   }
  };
}());

console.log(counter.increase()); //1
console.log(counter.increase()); //2

console.log(counter.decrease()); //1
console.log(counter.decrease()); //0
```

- 즉시 실행 함수가 반환하는 객체 리터럴은 즉시실행 함수의 실행 단계에서 평가되어 객체가 되고 객체의 메서드도 함수 객체로 생성됨.
- 객체 리터럴은 스코프 생성x
- 클로저인 increase, decrease 메서드의 상위스코프는 즉시실행함수의 렉시컬환경이다.

```
// 생성자 함수
const Counter = (function() {
 let num = 0;
  function Counter() {
   //this.num = 0;
  }

  Counter.prototype.increase = function () {
   return ++num;
  };

  Counter.prototype.decrease = function() {
   return num > 0 ? --num : 0;
  };

  return Counter;
}());

const Conunter = nuw Counter();

console.log(counter.increase()); //1
console.log(counter.increase()); //2

console.log(counter.decrease()); //1
console.log(counter.decrease()); //0
```

- num은 생성할 인스턴스의 프로퍼티가 아니라 즉시 실행함수에서 선언된 변수이므로 인스턴스로 접근x
- 생성자 함수 Counter는 프로토타입을 통해 increase, decrease메서드를 상속받는 인스턴스를 생성.
- increase, decrease 메서드는 즉시 실행 함수의 렉시컬 환경을 기억하는 클로저
- num 변수의 값은 increase, decrease 메서드만 변경 가능

### 함수형 프로그래밍에서 활용

```
//고차함수
function makeCounter(predicate) {
 let counter = 0;

 //클로저를 반환
 return function () {
  counter = predicate(counter);
  return counter;
 };
}
//보조함수
function increase(n) {
 return ++n;
}

//보조함수
function decrease(n) {
 return --n;
}

const increaser = makeCounter(increase);
console.log(increaser()); //1
console.log(increaser()); //2

const decreaser = makeCounter(decrease);
console.log(decreaser()); // -1
console.log(decreaser()); // -2
```

- makeCounter 함수는 인자로 받은 보조함수를 합성하여 반환하는 함수의 동작을 변경할 수 있다.
- makeCounter 함수를 호출해 반환된 함수는 자신만의 독립된 렉시컬 환경을 갖는다.
- makeCounter 함수를 호출하면 makeCounter 함수의 실행 컨텍스트가 생성된다 -> 함수 객체를 생성하여 반환한 후 소멸 -> 전역 변수인 increaser에 할당 =>> makeCounter 실행컨텍스트 소멸되었지만 렉시컬 환경은 반환한 함수의 \[\[Environment\]\] 내부 슬롯에 의해 참조
- 전역 변수에 할당된 함수는 각각의 독립된 렉시컬 환경을 갖기 때문에 자유변수를 공유하지 않아 카운터의 증감이 연동되지 않는다.

### 증감이 가능한 카운터

```
const counter = (function () {
 let counter = 0;
 return function (predicate) {
  counter = predicate(counter);
  return counter;
 };
}());
//보조함수
function increase(n) {
 return ++n;
}
//보조함수
function decrease(n) {
 reuturn --n;
}

console.log(counter(increase)); //1
console.log(counter(increase)); //2

console.log(counter(decrease)); //1
console.log(counter(decrease)); //0
```

- 렉시컬 환경을 공유하는 클로저
- makeCounter 함수를 두 번 호출x

## 5\. 캡슐화와 정보 은닉

**캡슐화**

- 객체의 상태를 나타내는 프로퍼티와  프로퍼티를 참조하고 조작할 수 있는 메서드를 하나로 묶는 것을 의미
- 객체의 특정 프로퍼티, 메서드를 감출 목적으로 사용 => 정보 은닉

**정보 은닉**

- 구현 일부를 외부에 공개되지 않도록 감춰 객체 상태가 변경되는 것을 방지 => 정보 보호
- 객체 간의 상호 의존성, 결합도를 낮추는 효과

<u>자바스크립트는 public, private, protected와 같은 접근 제한자를 제공x => 프로퍼티, 메서드 외부 공개되어 있다.</u>

```
const Person = (function () {
 let _age = 0; //private

 //생성자 함수
 function Person(name, age) {
  this.name = name; //public
  _age = age;
 }
 //프로토타입 메서드
 Person.prototype.sayHi = function () {
  console.log(`Hi My name is ${this.name}. I am ${_age}.`);
 };
 //생성자 함수 반환
 return Person;
}());

const me = new Person('Lee', 20);
me.sayHi(); //Hi! My name is Lee. I am 20.
console.log(me.name); //Lee
console.log(me._age); //undefined

const me = new Person('Kim', 30);
me.sayHi(); //Hi! My name is Kim. I am 30.
console.log(me.name); //Kim
console.log(me._age); //undefined

me.sayHi(); //Hi! My name is Lee. I am 30.
// _age 값이 변경됨.
```

- Person 생성자 함수, sayHi 메서드는 소멸한 즉시실행 함수의 지역변수 \_age를 참조할 수 있는 클로저이다.
- Person 생성자 함수가 여러개 인스턴스를 생성할 경우 \_age 변수의 상태가 유지되지 않는다. => sayHi 메서드가 단 한번 생성되는 클로저이기 때문에 발생하는 현상, 어떤 인스턴스가 호출해도 동일한 상위 스코프 사용

\=> 인스턴스 메서드를 사용하면 private 가능하지만 프로토타입 메서드를 사용하면 불가능해짐.

\=> private 필드 정의할 수 있는 새로운 표준 사양 제안

## 6\. 자주 발생하는 실수

### for문에 var키워드로 선언했을 때

\=> 함수 레벨 스코프를 갖기 때문에 i는 전역 변수, funcs 배열의 요소로 추가한 함수를 호출하면 i를 참조하여 i의 값 3 출력

```
var funcs = [];

for (var i = 0; i < 3; i++) {
 funcs[i] = function () { return i; } ;
}

for (var j = 0; j < funcs.length; j++) {
 console.log(funcs[j]());
}
```

### 해결 방법

1\. 즉시실행 함수안에 중첩함수 사용

2\. var 대신 let 키워드 사용 => 코드블록을 실행할 때마다 새로운 렉시컬 환경 생성, 식별자와 값을 등록하여 유지

3\. 고차 함수 사용 => 변수와 반복문의 사용 억제
