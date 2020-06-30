# TIL

UI Interaction Senior, 2020 TIL (Today I Learned)

## 2주차

<details>

<summary>6일차</summary>

### 함수

#### 함수 정의 및 호출, 실행

```javascript
// function
function fnName() {
  return '함수 정의';
}

//이름이 없는 함수 (표현식)
var fnName2 = function () {
  return '함수 정의2';
};

// 함수 호출 및 실행  실행연산자 : ()
fnName();
fnName2();
```

#### 함수 결과 반환 or 종료 (return)

```javascript
function includeString(str) {
  if (!str || typeof str !== 'string') {
    return;
  }
  return number;
}
```

함수는 기본적으로 undefined를 반환한다.

#### 함수 재귀

```javascript
function loop(n) {
  if (n > 15) {
    return;
  }
  loop(++n);
}
```

#### 함수 스택 (Last In First Out)

```javascript
function foo(i) {
  if (i < 0) {
    return;
  }
  console.log('begin:' + i);
  foo(i - 1); //재귀함수

  console.log('end :' + i); //재귀함수로 인한 일시정지
}
foo(3);
```

#### 다중중첩함수 (스코프체이닝은 성능 이슈를 만들수도 있으므로 주의를 요한다.)

#### this는 함수의 실행 주체이다.

#### 함수는 객체의 소유가 가능하다.

#### 전달인자 객체(arguments)

```javascript
function sum() {
  for (var total = 0, i = 0, l = arguments.length; i < l; i++) {
    total += arguments[i];
  }
  return total;
}

//arguments 전달인자 객체 (유사배열)

sum(12, 15, 18, 21, 24);

// 유사배열은 index, length 를 가지고 있어 배열처럼 사용가능하나 pop, push, shift, unshift등의 배열 메서드는 사용이 불가능하다.
```

#### 즉시실행함수 (IIFE)

```javascript
(function () {
  //코드 보호 (전역오염 방지)
})()(function (global, document) {
  //IIFE에 인자전달 -> 내부 매개변수 참조
  //스코프체이닝에 의한 성능문제 해결방법
})(window, window.document);
```

#### 함수 객체

속성과 메서드는 객체가 가질수 있는것 <br>
함수는 객체이다

###### 함수객체의 속성

<ul>
  <li>function.name (함수 자신의 이름)</li>  
  <li>function.length (전달인자 갯수) (arguments.length 를 더 많이 쓴다.)</li>
</ul>

###### 함수객체의 메서드

```javascript
function thisIsFunction(x, y, z) {
  console.log('this ->', this);
  console.log('x ->', x);
  console.log('y ->', y);
  console.log('z ->', z);
}

// .call()
thisIsFunction.call(document.head, 10, 20, 30);
// 함수를 실행시킬때 첫번째 인자로 this를 설정해줄수 있음

// .apply()
thisIsFunction.apply(doucment.body, [10, 20, 30]);
// 함수를 실행시킬때 첫번째 인자로 this를 설정해줄수 있음, 배열을 사용해서 인자 전달

// .bind()
thisIsFunction.bind(document.head, 10, 20, 30);
// 함수를 바로 실행시키지 않고 첫번째 인자로 this를 설정해 줄 수 있음
// 필요한 시점에 실행시킬수 있음 (이벤트에서 활용!)
```

</details>

<details open>

<summary>7일차</summary>

### [ES6] Arrow Function

function 키워드를 대신해서 간소화 한것이 화살표 함수이다.

```javascript
var sum = function (x, y) {
  return x + y;
};

const sum1 = (x, y) => {
  return x + y;
};

const sum2 = (x, y) => x + y;
//중괄호를 생략하면 retrun도 생략 가능하다.

const squared = n => n * n;
//매개변수가 1개일 경우 매개변수 괄호도 생략 가능하다.
```

```javascript
// 객체의 메서드에 화살표 함수를 사용할 경우
// this는 자신의 객체를 가리키는게 아니라 객체의 상위요소를 가르키게 된다.
// 메서드 내부에서의 화살표 함수를 쓸 경우 this는 객체를 가르킨다!

const videoGameConsole = {
  _name: 'PlayStation',
  _games: [],
  printGames: function () {
    console.log('this', this);
    this._games.forEach(game => {
      console.log('this', this);
    });
  },
};

videoGameConsole._games.push('갓오브워', '라스트오브어스', 'GTA');
videoGameConsole.printGames();

// this = videoGameConsole 객체

//메서드에 화살표 함수를 쓸 경우
const videoGameConsole = {
  _name: 'PlayStation',
  _games: [],
  printGames: () => {
    console.log(this); //window 객체
    this._games.forEach(game => {
      console.log(this); //this가 window 객체를 가리켜 forEach 참조 에러
    }, this);
  },
};

videoGameConsole._games.push('갓오브워', '라스트오브어스', 'GTA');
videoGameConsole.printGames();
```

결론:

- ES6를 사용하여 프로젝트를 진행한다면 함수표현식에서는 화살표 함수 적극 활용!
- 객체의 속성(메서드)로는 사용하지 말자!
- 객체 속성(메서드) 내부에서는 적극 활용하자

### Default Parameters

ES6는 함수의 매개변수의 기본값을 설정 할수 있다

```javascript
//ES5
function calcPayment(price, tax, discount) {
  if (!price) {
    throw new Error('price 매개변수 값은 필수입니다.');
  }
  tax = tax || 0.1;
  discount = discount || 0;
  return Math.floor(price * (1 + tax) - (1 - discount));
}

calcPayment(10000, 0.3, 0.1);

//ES6

function isRequired(param) {
  throw new Error(`${param} 매개 변수값은 필수 입니다.`);
}
function calcPayment(price = isRequired('price'), tax = 0.1, discount = 0) {
  return Math.floor(price * (1 + tax) - (1 - discount));
}

calcPayment();

// ES6 비구조 할당
function calcPayment({ price = isRequired('price'), tax = 0.1, discount = 0 }) {
  return Math.floor(price * (1 + tax) - (1 - discount));
}

calcPayment({ price: 30000 });
```

### Rest Parameters (나머지 매개변수)

함수의 나머지 매개변수를 하나로 모아 배열값으로 사용 가능하다.

```javascript
function sum(r = 0, ...nums) {
  nums.forEach(n => (r += n));
  return r;
}

console.log(sum(0, 1, 3, 10)); //0을 제외한 나머지 매개변수
```

### Spread Operator (전개 연산자)

```javascript
// ES5

// 배열복제
var oddNumber = [1, 3, 5, 7];
for (var copyOddNumber = [], i = 0, l = oddNumber.length; i < l; i++) {
  copyOddNumber[i] = oddNumber[i];
}
console.log(copyOddNumber); // [1, 3, 5, 7];

var copyOddNumberMap = oddNumber.map(function (number) {
  return number;
});
console.log(copyOddNumberMap); // [1, 3, 5, 7];

var copyOddNumberSlice = oddNumber.slice();
console.log(copyOddNumberSlice); // [1, 3, 5, 7];

//ES6
let oddNumber = [1, 3, 5, 7];
let copyOddNumberSpreadOperator = [...oddNumber]; // [1, 3, 5, 7]
```

```javascript
// 배열 결합
let oddNumber = [1, 3, 5, 7];
let evenNumber = [2, 4, 6, 8];

let numbers = [...oddNumber, ...evenNumber];
console.log(numbers); //[1,3,5,7,2,4,6,8]
```

</details>