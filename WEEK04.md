# TIL

UI Interaction Senior, 2020 TIL (Today I Learned)

## 4주차

<!-- 4주 1일차  -->
<details>

<summary>1일차</summary>

## 문서 프로그래밍 인터페이스

### DOM API (Document Object Model API)

자주 사용되는 DOM API

#### DOM 선택 API 메서드

- getElementById
- getElemnetsByTagName()
- getElementsByClassName() (IE 9+)
- querySelector() (IE 8+ CSS2 선택자로 제한, IE 9+)
- querySelectorAll()
- mathches() (IE9+ msMatcheSelctor() )

⚠️ querySelector(), querySelectorAll() 메서드로 수집된 집합은 라이브 상태가 아니다. 문서의 변경내용이 즉각 반영되지 않는다.

#### Node 속성

Node는 HTML을 구성하는 모든것을 말한다. (html요소, 빈줄, 공백 등등)

- childNodes: 특정요소의 직계 자식노드들을 수집 (children: 자식요소 노드들 수집)
- firstChild: 첫번째 자식노드
- lastChild: 마지막 자식노드
- nextSibling: 다음에 인접한 형제노드 (nextElementSibling을 활용하여 다음 요소를 찾자)
- previousSibling: 이전에 인접한 형제노드 (previousElementSibling를 활용하여 이전 요소를 찾자)
- parentNode: 부모 노드
- nodeType
  - Node.ELEMENT_NODE: 1
  - Node.TEXT_NODE: 3
  - Node.CDATA_SECTION_NODE: 4
  - Node.PROCESSING_INSTRUCTION_NODE: 7
  - Node.COMMENT_NODE: 8
  - Node.DOCUMENT_NODE: 9
  - Node.DOCUMENT_TYPE_NODE: 10
  - Node.DOCUMENT_FRAGMENT_NODE: 11
- nodeName: 해당 요소의 태그 이름을 대문자로 반환
- nodeValue: 노드내용을 수집 (textContent를 더 활용 IE9+ 지원 )
</details>
<!-- // 4주 1일차  -->

<!-- 4주 2일차 -->
<details open>

<summary>2일차</summary>

## 문서 프로그래밍 인터페이스

### DOM API (Document Object Model API)

- Node.hasChildNodes() : 현재 노드에 자식노드를 가지고 있는지 Boolean 값을 반환한다. (⚠️ 공백이나 주석도 노드이므로 주의!)

```js
var bodyNode = document.querySelector('body');
bodyNode.hasChildNodes(); // true
```

- Document.createElement() : 지정한 tagName의 HTML요소를 생성한다. (tageName을 인식 못할경우 HTMLUnknownElement를 반환한다.)

```js
var divEl = document.createElement('div');
console.log(divEl); // <div></div>
```

- Document.createTextNode(): 텍스트 노드를 생성한다.

```js
var text = document.createTextNode('Text Node 생성'); // Text Node 생성
```

- Node.appendChild(): 특정한 부모노드의 마지막 자식으로 요소를 붙인다.

```js
var scripts = document.createElement('script');
document.body.appendChild(scripts);
```

- Node.insertBefore(): 참조된 노드 앞에 있는 노드에 자식 노드를 삽입한다. (첫번째 전달인자는 삽입하려는 노드, 두번째 전달인자는 삽입 기준이 되는 노드이다. )

```html
<!-- 실행 전 -->
<ul class="parent">
  <li>child</li>
  <li>child</li>
  <li>child</li>
  <li>child</li>
</ul>
```

```js
var parent = document.querySelector('.parent');
var newEl = document.createElement('li');
var newElContent = document.createTextNode('new child');
newEl.appendChild(newElContent);
parent.insertBefore(newEl, parent.firstChild);
```

```html
<!-- 실행 후 -->
<ul class="parent">
  <li>new child</li>
  <li>child</li>
  <li>child</li>
  <li>child</li>
  <li>child</li>
</ul>

<!-- parent.insertBefore(newEl, null); 
두번째 전달인자(기준)가 null 일 경우 요소 맨뒤에 삽입된다.
-->
```

- Element.setAttribute(): 요소에 속성을 추가

```js
document.body.setAttribute('class', 'body-close');
document.body.setAttribute('class', '');
```

- Node.removeChild(): 노드의 자식요소 삭제

```js
var ul = document.createElement('ul');

for (var i = 0, listArray = []; i < 5; i++) {
  listArray.push(document.createElement('li'));
}
listArray.forEach(function (list) {
  ul.appendChild(list);
});
```

- Node.replaceChild() : 특정노드의 자식노드를 다른노드로 교체

```js
var parentNode = document.createElement('span');
var oldContent = document.createTextNode('old old');
parentNode.appendChild(oldContent);

console.log(parentNode); // <span>'old old'</span>

var newContent = document.createTextNode('new new');
parentNode.replaceChild(newContent, oldContent);

console.log(parentNode); // <span>'new new'</span>
```

- Node.cloneNode() 이 메서드를 호출한 노드를 복제하고 반환한다.
  - 인자에 true를 넣을경우 자식 요소까지 복제된다
  - 이벤트까지 복제되지는 않는다.
  - 복제 할 요소에 id 속성이 있을경우 주의하자

```js
var originalNode = document.querySelector('head');
var clone = originalNode.cloneNode(true);

console.log(clone);
```

- CSSStyleDeclaration.cssText: 인라인스타일 설정을 텍스트로 반환
</details>

<!-- 4주 2일차 -->