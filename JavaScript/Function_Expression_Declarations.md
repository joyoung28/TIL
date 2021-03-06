# **함수**

### 함수란

일련의 과정을 문으로 구현하고 코드 블록으로 감싸서 하나의 실행 단위로 정의한 것.

입력을 받아 출력을 내보낸다.

- **매개변수** : 함수 내부로 입력을 전달받는 변수
- **인수**: 입력
- **반환값**: 출력
- **함수 정의** : 함수를 호출하기 이전에 전달받을 매개변수와 실행할 문들, 반환할 값을 지정하는 것. 정의된 함수는 자바스크립트 엔진에 의해 평가되어 함수 객체가 된다.

```
//함수 정의
function add(x, y) { //매개변수
 return x + y; //반환값
}
```

- **함수 호출** : 인수를 매개변수를 통해 전달하면서 함수 실행을 명시적으로 지시하면 반환값을 반환한다.

```
//함수 호출
var result = add(2, 5); //인수

//반환값 반환
console.log(result);
```

### 함수 사용하는 이유

- **코드의 재사용**이 효율적이다.
- **유지보수 편의성**
- **코드의 신뢰성**

### 함수 리터럴

함수는 객체 타입의 값이므로 함수 리터럴로 생성할 수 있다.

**일반객체와 함수객체의 차이점**

- 일반객체는 호출 x, 함수는 호출 o
- 함수객체만의 고유한 프로퍼티를 갖는다.

### 함수 정의의 네가지 방식

1.  함수 선언문
2.  함수 표현식
3.  Function 생성자 함수
4.  화살표 함수

### 함수 선언문

함수 선언문은 표현식이 아닌 문이다.

함수 선언문은 함수 이름을 생략할 수 없다.

```
//함수 선언문
function add(x, y) {
 return x + y;
}
```

함수 이름은 함수 내부에서만 호출이 가능하나 자바스크립트 엔진이 함수이름과 같은 식별자를 암묵적으로 생성하여 객체를 할당한다. 함수는 함수 이름으로 호출하는 것이 아니라 함수 객체를 가리키는 식별자로 호출하는 것이다.

```
var add = function add(x, y) {	//식별자 암묵적으로 생성
 return x + y;
};

console.log(add(2, 5)); //식별자
```

### 함수 표현식

자바스크립트의 함수는 일급객체이며 값처럼 변수에 할당할 수도 있고 프로퍼티 값, 배열의 요소가 될 수 있다.

이처럼 함수 객체를 변수에 할당한 것을 함수 표현식이라고 한다. 함수 이름 생략 가능

일급객체 : 값의 성질을 갖는 객체

```
//함수 표현식
var add = function(x, y) {
 return x + y;
};

console.log(add(2, 5));
```

#### **장점**

- 클로져로 사용
- 콜백으로 사용

### 함수 선언문과 함수 표현식의 차이점

**함수 호이스팅** : 함수 선언문이 코드의 선두로 끌어 올려진 것처럼 동작.

#### **변수 호이스팅과 함수 호이스팅의 차이점**

**공통점**

런타임 이전에 자바스크립트 엔진에 의해 실행되어 식별자 생성

**차이점**

함수 선언문을 통해 암묵적으로 생성된 식별자는 함수 객체로 초기화 하고

var 키워드로 선언된 변수는 undefined로 초기화한다.

함수 표현식은 변수에 할당되는 값이 함수 리터럴인 문이다.

그러므로 변수 할당문의 값은 런타임에 평가되므로 함수 표현식은 할당문이 실행되는 시점에 평가되어 함수 객체가 된다. 따라서 함수 표현식으로 함수 정의하면 함수 호이스팅이 아니라 변수 호이스팅이 발생한다.
