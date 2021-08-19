# 스프레드 문법

**ES6에서 도입되었으며 ...은 값들의 집합을 펄쳐서 개별적인 값들의 목록으로 만든다.**

### 특징

- 스프레드 문법을 사용할 수 있는 대상은 Array, String, Map, Set, DOM 컬렉션, arguments와 같이 for ...of문으로 순회할 수 있는 이터러블에 한정된다.
- 스프레드 문법은 값이 아니므로 결과를 값에 할당할 수 없다.

### 스프레드 문법을 사용하는 경우

- 함수 호출문의 인수 목록
- 배열 리터럴의 요소 목록
- 객체 리터럴의 프로퍼티 목록

## 함수 호출문의 인수 목록에서 사용

```
const arr = [1, 2, 3];

const max = Math.max(...arr); //3
```

Math.max 메서드는 가변 인자 함수이다. 메서드에 숫자가 아닌 배열을 인수로 전달하면 최대값을 구할 수 없으므로 NaN 반환. => 스프레들 문법을 사용하면 간결하고 가독성이 좋다.

## 배열 리터럴의 요소 목록에서 사용

### concat

- ES5에서 2개의 배열을 결합하고 싶은 경우 concat 메서드 사용
- 스프레드 문법을 사용하면 별도의 메서드 사용x

```
//ES5
var arr = [1, 2].concat([3, 4);
console.log(arr); //[1, 2, 3, 4]

//ES6
const arr = [ ...[1, 2], ...[3, 4]];
console.log(arr); //[1, 2, 3, 4]
```

### splice

- ES5에서 배열 중간에 요소 추가, 제거하고 싶은 경우 splice 메서드 사용
- 세 번째 인수 \[2, 3\]을 2, 3으로 해체하여 전달하려면 apply메서드 호출
- 스프레드 문법 사용하면 간결하고 가독성 좋아진다.

```
//ES5
var arr1 = [1, 4];
var arr2 = [2, 3];

arr1.splice(1, 0, arr2);
console.log(arr1); //[1, [2, 3], 4]

//apply메서드 사용
Array.prototype.splice.apply(arr1, [1, 0].concat(arr2));
console.log(arr1) //[1, 2, 3, 4]

//ES6
arr1.splice(1, 0, ...arr2);
console.log(arr1); //[1, 2, 3, 4]
```

### 배열 복사

- ES5에서 배열을 복사하려면 slice 메서드 사용
- 원본 배열을 얕은 복사하여 새로운 복사본 생성

```
//ES5
var origin = [1, 2];
var copy = origin.slice();

console.log(copy); //[1, 2];
console.log(copy === origin); //false

//ES6
const origin = [1, 2];
const copy = [...origin];

console.log(copy); //[1, 2];
console.log(copy === origin); //false
```

### 이터러블을 배열로 변환

- ES5에서 apply 또는 call 메서드 사용하여 slice 메서드를 호출=>이터러블, 유사배열 객체 배열로 변환 가능
- ES6에서는 arguments 객체는 이터러블이면서 유사 배열 객체이므로 스프레드 문법을 사용할 수 있다.
- arguments 객체의 경우는 Rest 파라미터 사용하는 것이 더 낫다.
- 이터러블이 아닌 유사 배열 객체는 스프레드의 문법을 사용x

## 객체 리터럴의 프로퍼티 목록에서사용

```
//스프레드 문법
//객체 병합, 프로퍼티의 중복인 경우, 뒤에 위치한 프로퍼티가 우선권을 갖는다.
const merged = { ... { x: 1, y: 2 }, ... y: 10, z: 3 }};
console.log(merged); // {x: 1, y: 10, z: 3}

//특정 프로퍼티 변경
const changed = {... {x: 1, y: 2 }, y: 100 };
console.log(changed); // {x:1, y: 100}

//프로퍼티 추가
const added = {... {x: 1, y: 2 }, z: 100 }
console.log(added); // {x:1, y: 2, z: 100}
```
