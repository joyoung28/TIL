# 1\. this 키워드

메서드는 자신이 속한 객체의 상태, 즉 프로퍼티를 참조하고 변경 할 수 있어야한다.

자신이 속한객체의 상태를 참조하려면 먼저 자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.

### 객체 리터럴 방식으로 생성한 객체의 경우 

메서드 내부에서 메서드 자신이 속한 객체를 가리키는 식별자를 재귀적으로 참조가능

\=> 메서드가 호출되는 시점에는 이미 객체 리터럴의 평가가 완료되어 객체 생성, 식별자에 생성된 객체가 할당된 이후 이므로 메서드 내부에서 객체 식별자를 참조할 수 있다.

<u>but 자기 자신이 속한 객체를 재귀적으로 참조하는 방식은 일반적이지 않으며, 바람직하지 않다.</u>

### 생성자함수 방식으로 인스턴스를 생성하는 경우

생성자 함수 내부에서 프로퍼티 또는 메서드를 추가하기 위해 자신이 생성할 인스턴스를 참조할 수 있어야 한다. 생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수가 존재해야 한다.

\=> 생성자 함수를 정의하는 시점에는 인스턴스를 생성하기 이전이므로 생성자 함수가 생성할 인스턴스를 가리키는 식별자를 알 수 없다.

<u>자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 특수한 식별자(this)가 필요하다.</u>

### this

**this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수이다. this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조 할 수 있다.**

**특징**

- this는 자바스크립트 엔진에 의해 암묵적으로 생성, 코드 어디서든 참조 가능
- this 바인딩은 함수 호출 방식에 의해 동적으로 결정.

\*바인딩 : 식별자와 값을 연결하는 과정

# 2\. 함수 호출 방식과 this바인딩

<table style="border-collapse: collapse; width: 100%;" border="1" data-ke-align="alignLeft"><tbody><tr><td style="width: 50%; text-align: center;"><b>함수 호출 방식</b></td><td style="width: 50%; text-align: center;"><b>this 바인딩</b></td></tr><tr><td style="width: 50%;">일반 함수 호출</td><td style="width: 50%;">전역 객체&nbsp;</td></tr><tr><td style="width: 50%;">메서드 호출</td><td style="width: 50%;">메서드를 호출한 객체</td></tr><tr><td style="width: 50%;">생성자 함수 호출</td><td style="width: 50%;">생성자 함수가 생성할 인스턴스</td></tr><tr><td style="width: 50%;">Function.prototype.apply/call/bind 메서드에 의한 간접 호출</td><td style="width: 50%;">Function.prototype.apply/call/bind 메서드에 첫번째 인수로 전달한 객체</td></tr></tbody></table>

## 일반 함수 호출

**일반 함수로 호출하면 모든 함수(중첩, 콜백 함수 포함) 내부의 this에는 전역 객체가 바인딩된다. **

this는 객체의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수이므로 객체를 생성하지 않는 일반 함수에서는 this가 의미가 없다. => strict mode가 적용된 일반 함수 내부의 this에는 undefined가 바인딩 됨.

**문제점**

외부 함수인 메서드와 중접 함수 또는 콜백함수의 this가 일치 하지 않는다는 것은 중첩 함수 또는 콜백함수를 헬퍼 함수로 동작하기 어렵게 만든다.

**메서드 내부의 this 바인딩을 메서드 this 바인딩과 일치시키기 위한 방법**

1\. this 바인딩 변수에 할당 후 대신 참조

2\. Function.prototype.apply/call/bind 메서드 제공

3\. 화살표 함수

## 메서드 호출

**메서드를 호출한 객체, 메서드 이름 앞의 마침표(.) 앞에 기술한 객체가 바인딩된다.**

**특징**

- 메서드는 다른 객체의 프로퍼티에 할당하는 것으로 다른 객체의 메서드가 될 수도 있고 일반 변수에 할당하여 일반 함수로 호출 될 수도 있다.

```
const anoterPerson = {
 name: 'Kim'
};

anotherPerson.getName = person.getName;

//getName 메서드를 호출한 객체는 anotherPerson
console.log(anotherPerson.getName()); //Kim

//메서드를 변수에 할당
const getName = person.getName;

// getName 메서드를 일반 함수로 호출
console.log(getName()); //''
//일반 함수로 호출된 getName 함수 내부의 this.name은 브라우저 환경에서 window.name과 같다.
```

- 프로토타입 메서드 내부에서 사용된 this도 해당 메서드를 호출한 객체에 바인딩된다.

```
function Person(name) {
 this.name = name;
}

Person.prototype.getName = function() {
 return this.name;
};

const me = new Person('Lee');

//getName메서드를 호출한 객체는 me
console.log(me.getName()); //Lee

Person.prototype.name = 'Kim';

//getName 메서드를 호출한 객체는 Person.prototype
console.log(Person.prototype.getName()); //Kim
```

## 생성자 함수 호출

생성자 함수 내부의 this에는 생성자 함수가 생성할 인스턴스가 바인딩된다.

```
function Circle(radius) {
 this.radius = radius;
 this.getDiameter = function () {
  return 2 * this.radius;
 };
}

const circle1 = new Circle(5);
const circle2 = new Circle(10);

console.log(circle1.getDiameter());
console.log(circle2.getDiameter());
```

생성자 함수 : 객체를 생성하는 함수. 생성자 함수 정의 후 new 연산자와 함께 호출.

## Function.prototype.apply/call/bind 메서드에 의한 간접 호출

**apply/call 메서드**는 this로 사용할 객체와 인수리스트를 인수로 전달받아 함수로 호출한다.

**apply 메서드** : 호출할 함수의 인수를 배열로 묶어 전달.

**call 메서드** : 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달.

```
function getThisBinding() {
 console.log(arguments);
 return this;
}

const thisArg = { a: 1 };

console.log(getThisBinding.apply(thisArg, [1, 2, 3]));
//Arguments(3) [1, 2, 3, callee: f, Symbol(symbol.iterator): f]
//{a: 1}
console.log(getThisBinding.call(thisArg, 1, 2, 3));
//Arguments(3) [1, 2, 3, callee: f, Symbol(symbol.iterator): f]
//{a: 1}
```

- 대표적인 용도는 arguments 객체와 같은 유사 배열 객체에 배열 메서드를 사용하는 경우이다.

```
function convertArgsToArray() {
 console.log(arguments);

 //argumnets 객체를 배열로 반환
 //slice를 인수 없이 호출하면 배열의 복사본 생성
 const arr = Array.prototype.slice.call(arguments);
 //const arr = Array.prototype.slice.apply(arguments);
 console.log(arr);

 return arr;
}

convertArgsToArray(1, 2, 3); //[1, 2, 3]
```

**bind메서드**는 함수를 호출하지 않고 this로 사용할 객체만 전달

- 메서드의 this와 메서드 내부의 중첩함수, 콜백 함수의 this가 불일치하는 문제를 해결한다.

```
const person = {
 name: 'Lee',
 foo(callback) {
  setTimeout(callback.bind(this), 100);
 }
};

Person.foo(function() {
 console.log(`Hi! my name is ${this.name}.`);
});
```
