# 디스트럭처링 할당 destructuring assignment(구조 분해 할당)

<u>이터러블 또는 객체를 비구조화하여 1개 이상의 변수에 할당하는 것.</u>

<u>이터러블 또는 객체 리터럴에서 필요한 값만 추출하여 변수에 할당할 때 유용</u>

## 배열 디스트럭처링 할당

**배열 디스트럭처리 할당의 대상은 이터러블이며, 할당 기준은 인덱스이다. 순서의미o**

```
const arr = [1, 2, 3];

const [one, two, three] = arr;
console.log(one, two, three); // 1 2 3
```

- 할당 연산자 왼쪽에 할당받을 변수를 선언
- 변수를 배열 리터럴 형태로 선언
- 우변에 이터러블 할당하지 않으면 에러
- 선언과 할당을 분리할 수 있으나 const로 변수 선언x
- 변수에 기본값 설정 o
- 변수에 Rest요소를 사용할 수 있다. 마지막에 위치

## 객체 디스트럭처링 할당

**할당의 대상은 객체이며, 할당 기준은 프로퍼티 키이다. 순서의미x**

```
const user = { firstName: 'Ungmo', lastName: 'Lee' };
//디스트럭처링 할당
const { lastName, firstName } = user;
console.log(firstName, last Name); //Ungmo Lee

//다른 변수 이름으로 프러퍼티 값 할당
const { lastName: ln, firstName: fn } = user;

//변수에 기본값 설정
const { firstName = 'Ungmo', lastName } = { lastName: 'Lee' };

//배열의 요소가 객체인 경우 혼용
const todos = [
 { id: 1, content: 'HTML', completed: true},
 { id: 2, content: 'CSS', completed: false},
 { id: 3, content: 'JS', completed: false }
];

const [, { id }] = todos;
console.log(id) //2

//중첩 객체의 경우
const user = {
 name: 'Lee',
 address: {
  zipCode: '03068',
  city: 'Seoul'
 }
};

const { address: { city } } = user;
console.log(city); // 1 { y: 2, z: 3 }
```

- 변수를 객체 리터럴 형태로 선언
- 우변에 객체또는 객체로 평가되는 표현식을 할당하지 않으면 에러
- 변수에 기본값 설정
- 함수의 매개변수에도 사용할 수 있다.
- 배열의 요소가 객체인 경우 배열, 객체 디스트럭처링 할당을 혼용할 수 있다.
- Rest 프로퍼티를 사용할 수 있다. 마지막에 위치
