# ES6 함수의 추가 기능

## 1\. 함수의 구분

<u>ES6 이전에는 함수는 구분 없이 다양한 목적으로 사용되었다.</u>

모든 함수는 일반적인 함수, 생성자 함수로서 호출 할 수 있었다.

**문제점**

- 객체에 바인딩된 함수를 생성자 함수로 호출하는 경우 문법상은 가능하지만 성능 면에서 문제가 있다.
- 콜백함수도 constructor이기 때문에 불필요한 프로토타입 객체 생성.

\=> 사용 목적에 따라 명확한 구분이 없으므로 생성자 함수로 호출되지 않아도 프로토타입 객체를 생성한다.

<u>ES6 에서는 함수를 사용 목적에 따라 세 가지 종류로 명확히 구분했다.</u>

<table style="border-collapse: collapse; width: 100%;" border="1" data-ke-align="alignLeft"><tbody><tr><td style="width: 16.6667%;">ES6 함수 구분</td><td style="width: 16.6667%;">constructor</td><td style="width: 16.6667%;">prototype</td><td style="width: 16.6667%;">super</td><td style="width: 16.6667%;">arguments</td></tr><tr><td style="width: 16.6667%;">일반&nbsp; 함수(Nomal)</td><td style="width: 16.6667%;">O</td><td style="width: 16.6667%;">O</td><td style="width: 16.6667%;">X</td><td style="width: 16.6667%;">O</td></tr><tr><td style="width: 16.6667%;">메서드(Method)</td><td style="width: 16.6667%;">X</td><td style="width: 16.6667%;">X</td><td style="width: 16.6667%;">O</td><td style="width: 16.6667%;">O</td></tr><tr><td style="width: 16.6667%;">화살표 함수(Arrow)</td><td style="width: 16.6667%;">X</td><td style="width: 16.6667%;">X</td><td style="width: 16.6667%;">X</td><td style="width: 16.6667%;">X</td></tr></tbody></table>

## 2\. 메서드

<u>ES6 이전에는 객체에 바인딩된 함수를 의미</u>

<u>ES6 에서는 메서드는 메서드 축약 표현으로 정의된 함수를 의미</u>

```
const obj = {
 x: 1,
 //foo는 메서드
 foo() { return this.x; },
 //bar에 바인딩된 함수는 일반함수
 bar: function() { return this.x; }
};

console.log(obj.foo()); //1
console.log(obj.bar()); //1
```

**특징**

- 인스턴스를 생성할 수 없는 non-constructor이다. => 생성자 함수로서 호출x

```
new obj.foo(); // TypeError: obj.foo is not a constructor
new obj.bar(); // bar{}
```

- 자신을 바인딩한 객체를 가리키는 내부 슬롯 \[\[HomeObjectt\]\]를 갖는다. =>super참조는 내부 슬롯 \[\[HomeObject\]\]를 사용하여 수퍼클래스의 메서드를 참조.

```
const base = {
 name: 'Lee',
 sayHi() {
  return `Hi~ ${this.name}`;
 }
};

const derived = {
 __proto__ : base,
 //sayHi는 ES6 메서드이므로 [[HomeObject]]를 갖는다.
 //sayHi의 [[HomeObject]]는 derived.prototype을 가리키고
 //super는 sayHi의 [[HomeObject]]의 프로토타입인 base.prototype을 가리킨다.
 sayHi() {
  return `${super.sayHi()}. how are you doing?`;
 }
};

console.log(derived.sayHi()); //Hi~ Lee. how are you doing?
```

## 3\. 화살표 함수

function 키워드 대신 화살표를 사용하여 간략하게 함수를 정의한 것.

### **화살표 함수 정의**

**함수 정의** - 함수 표현식으로 정의

```
const multiply = (x, y) => x * y;
multiply(2, 3);
```

**매개변수 선언**

- 매개 변수가 여러 개인 경우 소괄호 안에 매개변수 선언
- 배개변수가 한개 인 경우 소괄호 생략 가능

**함수 몸체 정의**

- 함수 몸체 내부의 문이 값으로 평가될 수 있는 표현식이라면 중괄호 생략 가능, 암묵적으로 반환한다.
- 함수 몸체가 하나의 문으로 구성되어도 표현식이 아니면 중괄호 생략x
- 객체리터럴을 반환하는 경우 객체 리터럴을 소괄호로 감싸야 한다. => 객체리터럴의 중괄호를 함수 몸체를 감싸는 중괄호로 잘못 해석한다.
- 함수 몸체가 여러개의 문으로 구성되면 중괄호 생략x, 반환값을 명시적으로 반환.

```
const sum = (a, b) => {
 const result = a + b;
 return result;
};
```

- 즉시실행 함수로 사용 가능

```
const person = (name => ({
 sayHi() { return `Hi? My name is ${name}. `; }
}))('Lee');

console.log(person.sayHi());
```

- 콜백 함수로 정의할 때 유용하다.

### **화살표 함수와 일반 함사의 차이**

1.  **화살표 함수는 인스턴스를 생성할 수 없는 non-constructor이다.** => 화살표 함수는 인스턴스를 생성할 수 없으므로 prototype 프로퍼티가 없고 프로토타입도 생성x
2.  **중복된 매개변수를 선언할 수 없다.** =>일반 함수는 중복된 매개변수 이름을 선언해도 에러x(단, stric mode에서는 에러o)
3.  **화살표 함수는 함수 자체의 this, arguments, super, new.target 바인딩을 갖지 않는다.** => 화살표 함수 내부에서 this, arguments, super, new.target을 참조하면 스코프 체인 상에서 가장 가까운 상위 함수 중 화살표 함수가 아닌 함수를 참조한다.

### **this**

화살표 함수가 일반 함수와 구별되는 가장 큰 특징이다.

함수를 정의할 때 this에 바인딩할 객체가 정적으로 결정되는 것이 아니고, 함수를 호출할 때 함수가 어떻게 호출되었는지에 따라 this에 바인딩할 객체가 동적으로 결정된다.

**문제점**

<u>일반함수로 호출되는 콜백함수의 경우이다.</u>

```
class Prefixer {
 constructor(prefix) {
  this.prefix = prefix;
 }

 add(arr) {
  return arr.map(function (item) {
   return this.prefix + item;
   //TypeError: Cannot read property 'prefix' of undefined
  });
 }
}

const prefixer = new Prefixer('-webkit-');
console.log(prefixer.add(['transition', 'user-select']));
```

클래스 내부의 모든 코드는 stric mode 적용되어 일반 함수로서 호출된 모든 함수 내부의 this는 undefined가 바인딩된다. => 콜백함수의 this와 외부함수의 this가 다른 값을 가리키기 때문에 TypeError이다.

**ES6 이전 해결방법**

1\. add 메서드를 호출한 prefixer 객체를 가리키는 this를 재할당하여 재할당한 변수를 참조한다.

```
...
add(arr) {
 const that = this;
 return arr.map(function (item) {
  return that.prefix + '' + item;
 });
}
...
```

2\. Array.prototype.map의 두번째 인수로 add메서드를 호출한 prefixer 객체를 가리키는 this를 전달한다.

```
...
add(arr) {
 return arr.map(function (item) {
  return this.prefix + ' ' + item;
 }, this);
}
...
```

3\. Function.prototype.bind 메서드를 사용하여 add 메서드를 호출한 prefixer 객체를 가리키는 this를 바인딩한다.

```
...
add(arr) {
 return arr.map(function (item) {
  return this.prefix + ' ' + item;
 }.bind(this));
}
...
```

**ES6에서는 화살표 함수를 사용하여 해결할 수 있다.**

화살표함수는 함수 자체의 this 바인딩을 갖지않기 때문에 화살표 함수 내부에서 this를 참조하면 상위 스코프의 this를 그대로 참조 한다.

<u>화살표 함수가 전역 함수라면 화살표 함수의 this는 전역객체를 가리킨다.</u>

- 일반적인 의미의 메서드를 화살표 함수로 정의하는 것은 피해야한다. this는 메서드를 호출한 객체를 가리키지 않고 상위 스코프의 this인 전역 객체를 가리킨다.
  </br>=> ES6 메서드 축약 표현으로 정의한 ES6 메서드를 사용하는 것이 좋다.
- 프로토타입 객체의 프로퍼티에 화살표 함수를 할당하는 경우도 문제 발생한다.
  </br>=> 프로퍼티를 동적 추가하고 싶을 때는 일반 함수 할당한다, ES6메서드를 동적 추가하고 싶다면 객체 리터럴을 바인딩하고 프로토타입 constructor 프로퍼티와 생성자 함수간의 연결을 재설정한다.
- 클래스 필드 정의 제안을 사용하여 클래스 필드에 화살표 함수를 할당할 수 있다. but 클래스 필드에 할당한 화살표 함수는 프로토타입 메서드가 아니라 인스턴스 메서드가 된다.
  </br>=>ES6 메서드 축약 표현으로 정의한 ES6 메서드를 사용하는 것이 좋다.

### **super**

화살표 함수는 함수 자체의 super 바인딩을 갖지 않는다.

\=> 화살표 함수 내부에서 super를 참조하면 this와 마찬가지로 상위 스코프의 super를 참조

### **arguments**

<u>화살표 함수는 함수 자체의 arguments 바인딩을 갖지 않는다. 상위 스코프의 arguments를 참조</u>

**arguments객체** - 함수를 정의할 때 매개변수의 개수를 확정할 수 없는 가변 인자 함수를 구현할 때 유용

상위 스코프의 arguments 객체를 참조할 수 있지만 자신에게 전달된 인수 목록을 확인할 수 없고 상위 함수에게 전달된 인수 목록을 참조한다 => 화살표 함수로 가변 인자 함수를 구현해야 할 때는 **Rest 파라미터** 사용

## 4\. Rest 파라미터

### **기본 문법**

매개변수 이름 앞에 ...을 붙여서 정의한 매개변수

\=> Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달.

```
function foo(...rest) {
 console.log(rest); //[ 1, 2, 3, 4, 5]
}

foo(1, 2, 3, 4, 5)
```

**특징**

- 일반 매개변수와 Rest파라미터는 함께 사용할 수 있다. 함수에 전달된 인수들은 매개변수와 Rest 파라미터에 순차적으로 할당
- 먼저 선언된 매개변수에 할당된 인수를 제외한 나머지 인수들로 구성된 배열이 할당된다. =>Rest 파라미터는 반드시 마지막 파라미터여야 한다.
- 단 하나만 선언할 수 있다.
- 함수 정의 시 선언한 매개변수 개수를 나타내는 함수 객체의 length 프로퍼티에 영향x

### **Rest파라미터와 arguments 객체**

- ES5 => 가변 인자 함수의 경우 arguments 객체를 활용하여 인수를 전달받았으나  arguments 객체는 배열이 아닌 유사 배열 객체이므로 Function.prototype.call 이나 Function.prototype.apply 메서드를 사용해 arguments 객체를 배열로 반환.

```
function sum() {
 var array = Array.prototype.slice.call(arguments);

 return array.reduce(function (pre, cur) {
  return pre + cur;
 }, 0);
}

console.log(sum(1, 2, 3, 4, 5)); //15
```

- ES6 => Rest 파라미터를 사용하여 가변 인자 함수의 인수 목록을 배열로 직접 전달. 화살표함수는 arguments 객체를 갖지 않기 때문에 가변 인자 함수를 구현할 때는 무조건 Rest 파라미터를 사용해야한다.

```
function sum(...args) {
 return args.reduce((pre, cur) => pre + cur, 0);
}

console.log(sum(1, 2, 3, 4, 5)); //15
```

## 5\. 매개변수 기본값

함수가 호출할 때 매개변수의 개수만큼 인수를 전달하지 않아도 에러x

\=> 자바스크립트 엔진이 매개변수와 인수의 개수를 체크하지 못하기 때문

<u>인수가 전달되지 않을 경우 매개변수에 기본값을 할당할 필요가 있다.</u>

**장점**

함수 내에서 수행하던 인수 체크 및 초기화를 간소화한다.

**특징**

- 매개변수에 인수를 전달하지 않은 경우와 undefined를 전달한 경우에만 유효

- Rest 파라미터에는 기본값을 지정x

- 함수 객체의 length 프로퍼티와 arguments 객체에 아무런 영향x
