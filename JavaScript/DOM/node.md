## 노드

#### HTML 요소와 노드 객체

HTML 요소 : HTML 문서를 구성하는 개별적인 요소 의미.

HTML 요소는 파싱되어 DOM을 구성하는 요소 노드 객체로 변환된다.

HTML 요소 간에 부자 관계가 형성되어 HTML 요소를 객체화한 모든 노드 객체들을 트리 자료구조로 구성한다.

```
<div class="greeting">Hello</div>

div => 요소 노드
class="greeting" => 어트리뷰트 노드
"Hello" => 텍스트 노드
```

**트리 자료구조**

노드들의 계층 구조(부자, 형제관계)를 표현하는 비선형 자료구조

하나의 최상위 노드에서 시작하며, 최상위 노드는 루트 노드, 자식 노드가 없는 노드를 리프 노드

노드 객체들로 구성된 트리자료구조를 DOM이라 하며 DOM 트리라고 부른다.

#### 노드 객체의 타입

노드 객체는 총 12개의 종류이며 상속 구조를 갖는다.

**중요한 4가지 노드 타입**

**document node**

- 루트 노드로서 document 객체를 가리킨다.
- 전역 객체 window의 document 프로퍼티에 바인딩 되어있어 window.document 또는 document 참조 가능
- document 객체는 DOM 트리 노드에 접근하기 위한 진입점 역할

**element node**

- HTML 요소를 가리키는 객체
- 문서의 구조를 표현

**attribute node**

- HTML 요소의 어트리뷰트를 가리키는 객체
- 부모 노드가 없고 요소 노드에만 연결되어 있지만 요소 노드의 형제 노드는 아니다.
- 어트리뷰트 노드에 접근하여 참조, 변경하려면 요소 노드에 먼저 접근해야 한다.

**text node**

- HTML 요소의 텍스트를 가리키는 객체
- 문서의 정보를 표현
- 요소의 자식노드이며 리프 노드이다.
- 텍스트 노드에 접근하려면 요소 노드에 접근해야 한다.

#### 노드 객체의 상속 구조

노드 객체는 브라우저 환경에서 추가적으로 제공하는 호스트 객체이지만, 자바스크립트 객체이므로 프로토타입에 의한 상속 구조를 갖는다.

[##_Image|kage@7o7Za/btrdprdR9bC/Z6V7VGmAecjc10aQ0kCGBk/img.png|alignCenter|data-origin-width="3220" data-origin-height="1204" data-filename="node.png" data-ke-mobilestyle="widthOrigin"|||_##]

- 모든 노드 객체는 Object, EventTarget, Node 인터페이스를 상속받는다.
- 요소 노드 객체는 HTML 요소의 종류에 따라 고유한 기능을 가진다.
- 노드 객체는 상속을 통해 노드 타입에 따라 프로퍼티와 메서드의 집합인 DOM API를 사용하여 동적으로 조작할 수 있다.

### 1\. 요소 노드 취득

HTML의 구조 등을 동적으로 조작하려면 제일 먼저 요소 노드를 취득해야 한다.

#### id

**Document.prototype.getElmentById 메서드**

인수로 전달한 id를 갖는 하나의 요소 노드 탐색하여 반환. 반드시 document를 통해 호출해야 한다.

```
<!DOCYPE html>
<html>
 <body>
  <ul>
   <li id="apple">Apple</li>
   <li id="banana">Banana</li>
   <li id="orange">Orange</li>
  </ul>
  <script>
   const $elem = document.getElementById('banana');
   $elem.style.color = 'red';
  </script>
 </body>
</html>
```

- id 값을 갖는 요소가 여러 개이면 첫 번째 요소 노드만 반환한다. 하나의 요소 노드를 반환.
- id 값을 갖는 요소가 존재하지 않으면 null 반환
- HTML 요소에 id 어트리뷰트를 부여하면 id 값과 동일한 이름의 전역 변수가 암묵적 선언. 해당 노드 객체가 할당됨.
- id 값과 동일한 이름의 전역 변수가 선언되어 있으면 이 변수에 노드 객체 재할당 x

#### 태그 이름

**Document.prototype/Element.prototype.getElementsByTagName 메서드**

인수로 전달한 태그 이름을 갖는 모든 요소 노드들을 탐색하여 반환, 여러 개의 노드 객체를 갖는 HTMLCollection 객체를 반환.

```
<!DOCYPE html>
<html>
 <body>
  <ul>
   <li id="apple">Apple</li>
   <li id="banana">Banana</li>
   <li id="orange">Orange</li>
  </ul>
  <script>
   const $elem = document.getElementsByTagName('li');
   [...$elem].forEach(elem => { elem.style.color = 'red' });
  </script>
 </body>
</html>
```

- 모든 요소 노드를 취득하려면 인수로 \* 전달.
- Document.prototype 에서 정의된 메서드는 document를 통해 호출하며 DOM 전체에서 요소노드를 탐색하여 반환.
- Element.prototype 에서 정의된 메서드는 특정 요소 노드 통해 호출하며 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색하여 반환.
- 인수로 전달된 태그 이름 존재x => 빈 HTMLCollection 객체를 반환

#### class

**Documenet.prototype/Element.prototype.getElementsByClassName 메서드**

인수로 전달한 class 어트리뷰트 값을 갖는 모든 요소 노드들을 탐색하여 반환, 여러 개의 노드 객체를 갖는 HTMLCollection 객체를 반환.

```
<!DOCYPE html>
<html>
 <body>
  <ul>
   <li class="fruit apple">Apple</li>
   <li class="fruit banana">Banana</li>
   <li class="fruit orange">Orange</li>
  </ul>
  <script>
   const $elem = document.getElementsByClassName('fruit');
   [...$elem].forEach(elem => { elem.style.color = 'red' });

   const $apples = document.getElementsByClassName('fruit apple');
   [...$apples].forEach(elem => { elem.style.color = 'blue' });
  </script>
 </body>
</html>
```

- Document.prototype에 정의된 메서드는 document를 통해 호출하며 DOM 전체에서 요소 노드 탐색하여 반환
- Element.prototype 메서드는 특정 요소 노드를 통해 호출하며 특정 요소 노드의 자손 노드 중에서 요소 노드 탐색하여 반환.
- class 값을 갖는 요소 존재x => 빈 HTMLCollection 객체 반환.

####  CSS 선택자

#### 특정 요소 노드를 취득할 수 있는 지 확인

#### HTMLCollection 과 NodeList

2\. 노드 탐색

3\. 노드 정보 취득

4\. 요소 노드의 텍스트 조작
