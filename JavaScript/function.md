# **함수**

### 함수 호출

식별자와 한쌍의 소괄호인 함수 호출 연산자로 호출.

#### 매개변수와 인수

**인수** : 값으로 평가될 수 있는 표현식, 함수를 호출할 때 지정, 개수와 타입 제한x

**매개변수**  : 함수를 정의할 때 선언하며 함수 몸체 내부에서 암묵적으로 매개변수를 생성하여 변수와 동일하게 취급.

- 매개변수의 스코프는 함수 내부
- 인수가 부족해서 인수가 할당되지 않은 경우 매개변수의 값은 undefined.
- 매개변수보다 인수가 많은 경우 초과된 인수는 무시. ( 초과된 인수는 arguments 객체의 프로퍼티로 보관)

#### 인수확인

인수를 확인하지 않으면 개발자의 의도와는 다르게 자바스크립트 엔진은 아무런 이의 제기없이 코드를 실행할 것이다.

그러므로 함수를 정의할 때 적절한 인수가 전달되었는지 확인할 필요가 있다.

**인수확인을 하는 이유**

- 매개변수와 인수의 개수가 일치하는지 확인하지 않는다.
- 자바스크립트는 동적타입 언어이므로 매개변수 타입을 사전에 지정할 수 없다.

**인수확인 방법**

1\. 인수의 타입이 부적절한 경우 에러 발생

```
function add(x, y) {
 if (typeof x !== 'number' || typeof y !== 'number') {
  throw new TypeError('인수는 모두 숫자 값이어야 합니다.');
 }
 return x + y;
}

console.log(add(2));	//TypeError: 인수는 모두 숫자 값이어야 합니다.
console.log(add('a','b')); //TypeError: 인수는 모두 숫자 값이어야 합니다.
```

2\. arguments 객체를 통해 인수 개수 확인

3\. 인수가 전달되지 않은 경우 단축평가를 사용하여 매개변수에 기본값 할당

```
function add(a, b, c) {
 a = a || 0;
 b = b || 0;
 c = c || 0;
 return a + b + c;
}

console.log(add(1, 2, 3)); //6
console.log(add(1, 2)); //3
console.log(add(1)); //1
console.log(add()); //0
```

4\. 매개변수 기본값 사용하여 인수체크및 초기화를 간소화

\=> 매개변수에 인수를 전달하지 않았을 경우와 undefined를 전달한 경우에만 유효.

```
function add(a = 0, b = 0, c = 0) {
 return a + b + c;
}

console.log(add(1, 2, 3)); //6
console.log(add(1, 2)); //3
console.log(add(1)); //1
console.log(add()); //0
```

#### 매개변수의 최대 개수

- 함수는 한가지 일만 해야하며 가급적 작게 만들어야 하므로 매개변수는 최대 3개 이상을 넘지 않는 것을 권장한다.
- 그 이상 필요하면 객체를 인수로 전달하는 것이 유리 =>함수 내부로 전달한 객체를 함수 내부에서 변경하면 함수 외부의 객체가 변경되는 부수효과 발생(참조에 의한 전달, 외부상태의 변경)

#### 반환문

return 키워드 + 표현식

**반환문의 역할**

- 반환문은 함수의 실행을 중단하고 함수 몸체를 빠져나간다. => 반환 이후에 다른 문이 존재하면 그 문은 실행되지 않고 무시
- 리턴 키워드 뒤에 오는 표현식을 평가해 반환 =>반환값으로 사용할 표현식을 명시적으로 지시하지 않으면 undefined로 반환

**특징**

- 반환문 생략가능
- 반환문은 함수 몸체 내부에서만 사용 가능 => 전역에서 사용하면 문법 에러 발생

### 참조에 의한 전달과 외부상태의 변경

**원시 타입 인수** - 값자체가 복사되어 매개변수에 전달, 함수 몸체에서 값을 변경해도 원본값 훼손x

**객체 타입 인수** - 객체값을 참조하여 매개 변수에 전달, 함수 몸체에서 값을변경하면 참조값을 통해 원본객체를 훼손o

**객체 타입 인수를 사용하면 문제점**

함수가 외부상태를 변경하면 상태변화를 추적하기 어려워진다.

\=> 코드의 복잡성, 가독성을 해침

**해결방법**

- 객체를 불변객체로 만들어 변경 불가능한 값으로 만든다.
- 객체의 상태 변경이 필요한 경우에는 깊은 복사를 통해 새로운 객체를 생성하고 재할당을 통해 교체한다.           => 순수함수(외부상태 변경x, 의존x)

### 다양한 함수의 형태

#### 1\. 즉시 실행 함수

함수 정의와 동시에 즉시 호출, 단 한번만 호출

```
//익명 즉시 실행 함수
(function () {
 var a = 3;
 var b = 5;
 return a * b;
}());
```

**특징**

- 익명함수를 주로 사용
- 그룹연산자(...)로 감싸야한다. =>함수리터럴을 평가해서 함수 객체를 생성
- 일반 함수처럼 값을 반환할 수 있고 인수를 전달할 수도 있다.
- 즉시실행 함수 내에 코드를 모아 두면 변수나 함수 이름의 충돌을 방지할 수 있다.

#### 2\. 재귀 함수

함수가 자기자신을 호출하는 행위, 재귀 호출을 수행하는 함수.

```
//함수 표현식
var factorial = function foo(n) {
 //탈출 조건
 if (n <= 1) return 1;
 //식별자로 재귀 호출
 return n * factorial(n - 1);
};

console.log(factorial(5)); //5! = 5 * 4 * 3 * 2 * 1 = 120
```

**특징**

- 반복문 없이 반복되는 처리 구현
- 함수 내부에서는 함수 이름 또는 식별자로도 자기자신을 호출할 수 있다. (단, 함수외부에서는 식별자 호출)
- 재귀함수 내에는 재귀호출을 멈출 수 있는 탈출조건을 반드시 만들어야 한다. => 탈출조건 없으면 함수 무한 호출되어 스택오버플로 발생.
- 반복문을 사용하는 것보다 재귀함수를 사용하는 것이 직관적으로 이해하기 쉬울 때 사용한다.

#### 3\. 중첩 함수

함수 내부에 정의된 함수

중첩함수는 중첩함수를 포함하는 외부함수 내에서만 호출 가능

```
function outer() {
 var x = 1;

 //중첩함수
 function inner() {
  var y = 2;
  //외부 함수의 변수 참조 가능
  console.log(x + y); //3
 }

  inner();
}

outer();
```

#### 4\. 콜백 함수

함수의 매개변수를 통해 다른 함수의 내부로 전달되는 함수

**고차함수** : 매개변수를 통해 함수의 외부에서 콜백함수를 전달 받은 함수, 반환값으로 함수를 반환하는 함수.

```
function repeat(n, f) {
 for (var i = 0; i < n; i++) {
  f(i);
 }
}

var logAll = function (i) {
 console.log(i);
};

repeat(5, logAll); //0 1 2 3 4

var logOdds = function (i) {
 if (i % 2) console.log(i);
};

repeat(5, logOdds); //1 3
```

**장점**

함수 외부에서 고차함수 내부로 주입하기 때문에 함수 교체 가능

**콜백함수의 정의**

고차함수 내에 정의한 콜백함수는 고차함수가 호출될 때마다 평가되어 함수 객체를 생성한다.

콜백함수를 다른 곳에서도 호출하거나 고차함수가 자주 호출된다면 함수 외부에서 콜백함수를 정의한 후 함수 참조를 전달하는 편이 효율적이다.

**콜백함수 사용**

함수형 프로그래밍 패러다임뿐만 아니라 비동기 처리, 배열 고차 함수에도 사용된다.

#### 5\. 순수 함수와 비순수 함수

**순수 함수** : 외부 상태에 의존하지도 않고 변경하지도 않는 함수, 동일한 인수가 전달되면 언제나 동일한 값을 반환

```
var count = 0;

// 순수 함수는 동일한 값이 전달되면 동일한 값 반환
function increase(n) {
 return ++n;
}

// 순수 함수가 반환한 결과값을 변수에 재할당해서 상태 변경
count = increase(count);
console.log(count); //1

count = increase(count);
console.log(count); //2
```

**비순수 함수** : 외부 상태에 의존하거나 외부상태를 변경하는 함수, 매개변수를 통해 객체를 전달받은 경우

```
var count = 0;

// 비순수 함수
function increase() {
 return ++count; //외부상태 의존
}

//외부상태를 변경하므로 상태 변화 추적하기 어려움
increase();
console.log(count); //1

increase();
console.log(count); //2
```

함수형 프로그래밍은 수수 함수와 보조 함수의 조합을 통해 외부상태를 변경하는 부수 효과를 최소화해서

불변성을 지향하는 프로그래밍 패러다임이다.