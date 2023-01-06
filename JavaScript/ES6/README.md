# JavaScript ES6 문법

- const, let
- 화살표 함수
- 객체 리터럴
- 템플릿 리터럴
- 구조분해할당
- Spread, Rest
- Default parameter
- for..of 문
- 모듈
- Class
- Promise
- Symbol(7번째 타입 심볼)
- async/await

## 1️⃣ const, let

### ✔ let

1. 변수 중복 선언 금지

```js
let bar = 123;
let bar = 456; //SyntaxError: Identifier 'bar' has already been declared.
```

2. 블록 레벨 스코프

- var 키워드로 선언한 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정하는 함수 레벨 스코프를 따른다.
- let 키워드로 선언한 변수는 모든 코드 블록을 지역 스코프로 인정하는 블록 레벨 스코프를 따른다.

```js
let foo = 1; //전역변수

{
  let foo = 2; //지역변수
  let bar = 3; //지역변수
}

console.log(foo); //1
console.log(bar); //ReferenceError: bar is not declared
```

3. 변수 호이스팅

- var 키워드로 선언한 변수와 달리 let 키워드로 선언한 변수는 호이스팅이 발생하지 않는 것처럼 동작한다.
- 호이스팅 : 코드가 실행하기 전 변수선언/함수선언이 해당 스코프의 최상단으로 끌어 올려진 것 같은 현상을 말함

```js
console.log(foo); //ReferenceError: foo is not declared
let foo;
```

- var 키워드로 선언한 변수는 런타임 이전에 자바스크립트에 의해 암묵적으로 '선언 단계'와 '초기화 단계'가 진행된다. 따라서 변수 선언문 이전에 변수를 참조할 수 있다.

```js
console.log(foo); //undefined

var foo;
console.log(foo); //undefined

foo = 1; //할당문에서 할당 단계가 실행된다.
console.log(foo); //1
```

- let 키워드로 선언한 변수는 '선언 단계'와 '초기화 단계'가 분리되어 진행된다. 즉, 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 선언 단계가 먼저 실행되지만 초기화 단계는 변수 선언문에 도달했을 때 실행된다.
- 초기화 단계 이전에 변수에 접근하려고 하면 ReferenceError가 발생한다.
- TDZ(Temporal Dead Zone, 일시적 사각지대) : 스코프의 시작 지점부터 초기화 시작 지점까지 변수를 참조할 수 없는 구간을 뜻함

```js
// 런타임 이전에 선언 단계가 실행된다. 아직 변수가 초기화되지 않았다.
//초기화 이전의 일시적 사각지대에서는 변수를 참조할 수 없다.
console.log(foo); //ReferenceError: foo is not declared

let foo; // 변수 선언문에서 초기화 단계가 실행된다.
console.log(foo); //undefined

foo = 1; //할당문에서 할당 단계가 실행된다.
console.log(foo); //1
```

- 결국 let 키워드로 선언한 변수는 호이스팅이 발생하지 않는 것처럼 보이지만 사실은 그렇지 않다.

```js
let foo = 1; //전역 변수
{
  console.log(foo); //ReferenceError: Cannot access 'foo' before initialization
  let foo = 2; //지역 변수
}
```

호이스팅이 발생하지 않는다면 위의 예제는 전역 변수 foo의 값을 출력해야 한다. 하지만 let 키워드로 선언한 변수도 여전히 호이스팅이 발생하기 때문에 ReferenceError가 발생한다.

### ✔ const

- const 키워드는 상수를 선언하기 위해 사용한다.

1. 선언과 초기화

- const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 한다.

```js
const foo; //SyntaxError: Missing initailizer in const declaration
```

2. 재할당 금지

```js
const foo = 1;
foo = 2; //TypeError: Assignment to constant vairable.
```

## 2️⃣ 화살표 함수

- function 키워드 대신 화살표(=>, fat arrow)를 사용하여 기존의 함수 정의 방식보다 간략하게 함수를 정의할 수 있다.
- '인자 => 함수 본문' 의 형태

### ✔ 화살표 함수와 일반 함수의 차이

1. 화살표 함수는 인스턴스를 생성할 수 없는 non-constructor다. 즉, prototype 프로퍼티가 없고, 프로토타입도 생성하지 않는다.

```js
const func = () => {};
const temp = new func(); // Uncaught TypeError: func is not a constructor
```

2. 일반 함수는 중복된 매개변수 이름을 선언해도 에러가 발생하지 않지만(단, strict 모드 예외), 화살표 함수에서는 중복된 매개 변수 이름을 선언하면 에러가 발생한다.

```js
const arrow = (a, a) => a + a; //SyntaxError: Duplicate parameter name not allowed in this context
```

3. 화살표 함수는 함수 자체의 this, arguments, super, new.target 바인딩을 갖지 않는다. 따라서 화살표 함수 내부에서 this, arguments, super, new.target 을 참조하면 스코프 체인 상에서 가장 가까운 상위 함수 중 화살표 함수가 아닌 함수의 this, arguments, super, new.target을 참조한다.

## ✔ this

this 바인딩은 함수의 호출 방식에 의해 동적으로 결정된다.

```js
class Prefixer() {
  constructor(prefix) {
    this.prefix = prefix;
  }

  add(arr) {
    //add 메서드는 인수로 전달된 배열 arr을 순회하며 배열의 모든 요소에 prefix를 추가한다.
    return arr.map(function (item){
      return this.prefix + item; //TypeError: Cannot read property 'prefix' of undefined
    });
  }
}

const prefixer = new Prefixer('-webkit-');
console.log(prefixer.add(['transition','user-select']));
```

- 기대하는 결과는 ['-webkit-transition', '-webkit-user-select']지만 TypeError가 발생한다.
- Array.prototype.map 메서드가 콜백함수를 일반함수로 호출한다.
- 일반함수로 호출된 모든 함수의 내부의 this에는 전역객체(window)가 바인딩 된다.
- 그런데 클래스 내부의 모든 코드에는 strict mode가 암묵적으로 적용되므로 undefined가 바인딩 된다.
- '콜백 함수의 this와 외부 함수의 this가 서로 다른 값을 가리키고 있'기 때문에 TypeError가 발생했다.

### 💡 콜백 함수 내부의 this 문제를 해결하기 위한 방법(es6 이전)

1. this 회피시킨 후 콜백 함수 내부에서 사용하기

```js
add(arr) {
  const that = this;
  return arr.map(function (item) {
    //this 대신 that을 참조한다.
    return that.prefix + ' ' + item;
  })
}
```

2. Array.prototype.map의 두번째 인자로 this를 전달

```js
add(arr) {
  return arr.map(function (item) {
    return this.prefix + ' ' + item;
  }, this); //this에 바인딩된 값이 콜백 함수 내부의 this에 바인딩 된다.
}
```

3. Function.prototype.bind 메서드를 사용해서 this를 바인딩

```js
add(arr) {
  return arr.map(function (item) {
    return this.prefix+ ' ' + item;
  }.bind(this)); //this에 바인딩된 값이 콜백 함수 내부의 this에 바인딩된다.
}
```

### es6 에서는 화살표 함수를 사용하여 해결할 수 있다.

```js
class Prefixer() {
  constructor(prefix) {
    this.prefix = prefix;
  }

  add(arr) {
    //add 메서드는 인수로 전달된 배열 arr을 순회하며 배열의 모든 요소에 prefix를 추가한다.
    return arr.map(item => this.prefix + item);
  }
}

const prefixer = new Prefixer('-webkit-');
console.log(prefixer.add(['transition','user-select'])); //['-webkit-transition', '-webkit-user-select']
```

- 화살표 함수는 함수 자체의 this 바인딩을 갖지 않는다. 따라서 화살표 함수 내부에서 this를 참조하면 상위 스코프의 this를 그대로 참조한다.(lexical scope)

## 3️⃣ 객체 리터럴 강화

- 계산된 프로퍼티를 사용할 수 있음
- 프로퍼티 축약 표현 지원
- 메소드 축약 표현 지원
- `__proto__` 프로퍼티에 의한 상속

### ✔ 계산된 프로퍼티

```js
const num_01 = 1;
const num_02 = 2;
const strHello = 'hello';

const newObj = {
  [1 + 1]: 'first',
  [num_01 + num_02]: 'second',
  [strHello + num_01]: num_02,
  [`${strHello} - ${num_01 + num_02}`]: 'fourth',
};

console.log(newObj);
//{ '2': 'first', '3': 'second', hello1: 2, 'hello - 3': 'fourth' }
```

### ✔ 프로퍼티 축약 표현

- 프로퍼티 값으로 변수를 사용하는 경우, 프로퍼티 이름을 생략(Property shorthand)할 수 있다. 이때 프로퍼티 이름은 변수의 이름으로 자동 생성된다.

```js
// ES5
var x = 1,
  y = 2;

var obj = {
  x: x,
  y: y,
};

console.log(obj); // { x: 1, y: 2 }

// ES6
let x = 1,
  y = 2;

const obj = { x, y };

console.log(obj); // { x: 1, y: 2 }
```

### ✔ 메소드 축약 표현

- ES6에서는 메소드를 선언할 때, function 키워드를 생략한 축약 표현을 사용할 수 있다.

```js
// ES5
var obj = {
  name: 'Lee',
  sayHi: function () {
    console.log('Hi! ' + this.name);
  },
};

obj.sayHi(); // Hi! Lee

// ES6
const obj = {
  name: 'Lee',
  // 메소드 축약 표현
  sayHi() {
    console.log('Hi! ' + this.name);
  },
};

obj.sayHi(); // Hi! Lee
```

### ✔ `__proto__` 프로퍼티에 의한 상속

- ES5에서 객체 리터럴을 상속하기 위해서는 Object.create() 함수를 사용한다. 이를 프로토타입 패턴 상속이라 한다.

```js
// ES5
var parent = {
  name: 'parent',
  sayHi: function () {
    console.log('Hi! ' + this.name);
  },
};

// 프로토타입 패턴 상속
var child = Object.create(parent);
child.name = 'child';

parent.sayHi(); // Hi! parent
child.sayHi(); // Hi! child
```

- ES6에서는 객체 리터럴 내부에서 `__proto__` 프로퍼티를 직접 설정할 수 있다.

```js
// ES6
const parent = {
  name: 'parent',
  sayHi() {
    console.log('Hi! ' + this.name);
  },
};

const child = {
  // child 객체의 프로토타입 객체에 parent 객체를 바인딩하여 상속을 구현한다.
  __proto__: parent,
  name: 'child',
};

parent.sayHi(); // Hi! parent
child.sayHi(); // Hi! child
```

## 4️⃣ 템플릿 리터럴

```js
let user = {
  name: 'Ted',
  greeting() {
    console.log(`Hello, I'm ${this.name}.`);
  },
};

user.greeting(); // Hello, I'm Ted.
```

## 5️⃣ 구조분해할당(비구조화 할당)

### ✔ 배열 구조분해할당

- ES6의 배열 디스트럭처링은 배열의 각 요소를 배열로부터 추출하여 변수 리스트에 할당한다. 이때 추출/할당 기준은 배열의 인덱스이다.

```js
const arr = [1, 2, 3];

// 배열의 인덱스를 기준으로 배열로부터 요소를 추출하여 변수에 할당
const [one, two, three] = arr;

console.log(one, two, three); // 1 2 3
```

### ✔ 객체 구조분해할당

- ES6의 객체 디스트럭처링은 객체의 각 프로퍼티를 객체로부터 추출하여 변수 리스트에 할당한다. 이때 할당 기준은 프로퍼티 이름(키)이다.

```js
const object = { a: 1, b: 2 };

const { a, b } = object;

console.log(a); // 1
console.log(b); // 2
```

## 6️⃣ Rest, Spread

### Rest 파라미터

- Rest 파라미터(Rest Parameter, 나머지 매개변수)는 매개변수 이름 앞에 세개의 점 ...을 붙여서 정의한 매개변수를 의미한다.
- Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받는다.

```js
function foo(...rest) {
  console.log(Array.isArray(rest)); // true
  console.log(rest); // [ 1, 2, 3, 4, 5 ]
}

foo(1, 2, 3, 4, 5);
```

```js
function foo(param, ...rest) {
  console.log(param); // 1
  console.log(rest); // [ 2, 3, 4, 5 ]
}

foo(1, 2, 3, 4, 5);

function bar(param1, param2, ...rest) {
  console.log(param1); // 1
  console.log(param2); // 2
  console.log(rest); // [ 3, 4, 5 ]
}

bar(1, 2, 3, 4, 5);
```

### Spread 문법

- Spread 문법(Spread Syntax, ...)은 이터러블 요소를 개별 요소로 분리한다.

```js
// ...[1, 2, 3]는 [1, 2, 3]을 개별 요소로 분리한다(→ 1, 2, 3)
console.log(...[1, 2, 3]); // 1, 2, 3

// 문자열은 이터러블이다.
console.log(...'Hello'); // H e l l o

// Map과 Set은 이터러블이다.
console.log(
  ...new Map([
    ['a', '1'],
    ['b', '2'],
  ])
); // [ 'a', '1' ] [ 'b', '2' ]
console.log(...new Set([1, 2, 3])); // 1 2 3

// 이터러블이 아닌 일반 객체는 Spread 문법의 대상이 될 수 없다.
console.log(...{ a: 1, b: 2 });
// TypeError: Found non-callable @@iterator
```

#### ✔ 함수의 인수로 사용하기

```js
function foo(x, y, z) {
  console.log(x); // 1
  console.log(y); // 2
  console.log(z); // 3
}

const arr = [1, 2, 3];

foo(...arr);
```

#### ✔ 배열에서 사용하기

```js
const arr = [1, 2, 3];
// ...arr은 [1, 2, 3]을 개별 요소로 분리한다
console.log([...arr, 4, 5, 6]); // [ 1, 2, 3, 4, 5, 6 ]
```

## 7️⃣ Default parameter

```js
function addToCart(item, size = 1) {
  shoppingCart.push({ item: item, count: size });
}

addToCart('Apple');
addToCart('Orange', 2);
console.log(shoppingCart); // [{item: "Apple", count: 1}, {item: "Orange", count: 2}]
```

## for..of

## 모듈

## 클래스

## Promise

## Symbol

## async/await
