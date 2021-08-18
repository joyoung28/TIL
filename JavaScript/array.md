# 배열

## 배열이란?

**요소** : 배열이 가지고 있는 값

**인덱스** : 배열의 요소가 자신의 위치를 나타내는 0이상의 정수

**length 프로퍼티** : 배열의 길이

**특징**

- 요소에 접근할 때는 대괄호 표기법 사용.
- 자바스크립트 배열은 객체 타입이다.
- 일반 객체와 배열을 구분하는 차이

<table style="border-collapse: collapse; width: 100%;" border="1" data-ke-align="alignLeft"><tbody><tr><td style="width: 33.3333%; text-align: center;"><b>구분</b></td><td style="width: 33.3333%; text-align: center;"><b>객체</b></td><td style="width: 33.3333%; text-align: center;"><b>배열</b></td></tr><tr><td style="width: 33.3333%; text-align: center;">구조</td><td style="width: 33.3333%; text-align: center;">키와 값</td><td style="width: 33.3333%; text-align: center;">인덱스와 요소</td></tr><tr><td style="width: 33.3333%; text-align: center;">값의 참조</td><td style="width: 33.3333%; text-align: center;">키</td><td style="width: 33.3333%; text-align: center;">인덱스</td></tr><tr><td style="width: 33.3333%; text-align: center;">값의 순서</td><td style="width: 33.3333%; text-align: center;">x</td><td style="width: 33.3333%; text-align: center;">o</td></tr><tr><td style="width: 33.3333%; text-align: center;">length 프로퍼티</td><td style="width: 33.3333%; text-align: center;">x</td><td style="width: 33.3333%; text-align: center;">o</td></tr></tbody></table>

밀집 배열 : 요소가 하나의 데이터 타입으로 통일되어 있으며 연속적으로 인접해 있는 배열

희소 배열 : 메모리가 크기가 동일하지 않으며 요소가 연속적으로 이어져 있지 않는 배열

자바스크립트의 배열은 일반적인 배열의 동작을 흉내 낸 특수한 객체이다.

일반 배열은 인덱스로 빠르게 접근 가능, 특정 요소를 검색, 삽입, 삭제하는 경우 효율적x

자바스크립트 배열은 인덱스로 요소에 접근하게 되면 일반 배열보다 성능적인 면에서 느릴 수 있지만, 특정 요소 삽입, 검색, 삭제하는 경우 일반 배열보다 빠름.

\=> 인덱스로 요소에 접근할 경우 일반 배열보다 느린 구조적인 단점을 보완하기 위해 모던 자바스크립트 엔진은 배열을 일반 객체와 구분하여 배열처럼 동작하도록 최적화함.

\=> 자바스크립트는 희소배열을 문법적으로 허용. 하지만 희소 배열은 사용하지 않는 것이 좋다. 배열에는 같은 타입의 요소를 연속적으로 위치시키는 것이 좋다.

## 배열 생성

### 1\. 배열 리터럴

```
const arr = [1, 2, 3];
console.log(arr.length); //3

const arr = [];
console.log(arr.length); //0

const arr = [1, , 3]; //희소 배열
console.log(arr.length); //3
console.log(arr); //[1, empty, 3]
console.log(arr[1]); // undefined
```

### 2\. Array 생성자 함수

```
const arr = new Array(10);

console.log(arr); //[empty * 10], 희소배열
console.log(arr); //10

new Array(); //[]

Array(1, 2, 3); // [1, 2, 3]
```

### 3\. Array.of

```
Array.of(1); //[1]

Array.of(1, 2, 3); //[1, 2, 3]

Array.of('string'); //['string']
```

### 4\. Array.from

```
//유사 배열 객체를 변환하여 배열 생성
Array.from({ length: 2, 0: 'a', 1: 'b' }); //['a', 'b']

//이터러블 객체를 변환하여 배열 생성
Array.from('Hello'); // ['H', 'e', 'l', 'l', 'o']
```
