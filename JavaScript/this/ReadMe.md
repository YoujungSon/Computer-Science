### this란?

> JavaScript에서 함수의 `this` 키워드는 다른 언어와 조금 다르게 동작합니다.
> 대부분의 경우 `this`의 값은 함수를 **호출한 방법에 의해 결정**됩니다.

```jsx
// this는 함수가 호출될 때 결정이 된다.
const car = {
  name: 'KIA',
  getName: function () {
    console.log('car getName', this);
  },
};

car.getName(); // {name: 'KIA', getName: f} car가 getName()을 부름
```

```jsx
const globalCar = car.getName();
globalCar(); // window {...} 자바스크립트 내장객체 window가 globalCar()를 부름
```

```jsx
const car2 = {
  name: 'hyundai',
  getName: car.getName,
};
car2.getName(); // {name: 'hyundai', getName: f} car2가 getName()를 부름
```

```jsx
const btn = document.getElementById('button');
btn.addEventListener('click', car.getName); // <button id='button'>this는 누굴까?</button>
// btn이 car.getName을 호출
```

### .bind : this를 고정 시켜주기 위해 쓰는 함수

```jsx
const bindGetName = car2.getName.bind(car);
bindGetName(); // {name: 'KIA', getName: f}
```

```jsx
const btn = document.getElementById('button');
btn.addEventListener('click', car.getName.bind(car)); // {name: 'KIA', getName: f}
```

```jsx
const testCar = {
  name: 'benz',
  getName: function () {
    console.log('getName', this.name);
    const innerFunc = function () {
      console.log('innerFunc', this.name);
    };
    innerFunc();
  },
};

testCar.getName();
// getName benz
// innerFunc
```

```jsx
const testCar = {
  name: 'benz',
  getName: function () {
    console.log('getName', this);
    const innerFunc = function () {
      console.log('innerFunc', this);
    };
    innerFunc();
  },
};

testCar.getName();
// getName { name: 'benz', getName: f }
// innerFunc window{}
```

```jsx
// 화살표 함수에서의 this는 함수가 속해있는 곳의 상위 this를 계승받는다.
const testCar = {
  name: 'benz',
  getName: function () {
    console.log('getName', this);
    const innerFunc = () => {
      console.log('innerFunc', this);
    };
    innerFunc();
  },
};

testCar.getName();
// getName { name: 'benz', getName: f }
// innerFunc { name: 'benz', getName: f }
```

```jsx
const ageTest = {
  unit: '살',
  ageList: [10, 20, 30],
  getAgeLsit: function () {
    const result = this.ageList.map(function (age) {
      return age;
    });

    console.log(result);
  },
};

ageTest.getAgeLsit();
[10, 20, 30];
```

```jsx
// map 안에 파라미터로 들어가는 함수는 일반함수로써 호출되는 콜백함수로
// 일반함수의 경우 this가 window객체(전역객체)를 바라본다.
const ageTest = {
  unit: '살',
  ageList: [10, 20, 30],
  getAgeLsit: function () {
    const result = this.ageList.map(function (age) {
      return age + this.unit;
    });

    console.log(result);
  },
};

ageTest.getAgeLsit();
[NaN, NaN, NaN];
```

```jsx
const ageTest = {
  unit: '살',
  ageList: [10, 20, 30],
  getAgeLsit: function () {
    const result = this.ageList.map((age) => {
      return age + this.unit;
    });
    console.log(result);
  },
};

ageTest.getAgeLsit();
['10살', '20살', '30살'];
```

> ES5는 함수를 어떻게 호출했는지 상관하지 않고 this 값을 설정할 수 있는 bind 메서드를 도입했고, ES2015는 스스로의 this 바인딩을 제공하지 않는 **화살표 함수**를 추가했습니다.

### 결론

this를 쓰고 싶다면 화살표 함수보다는 일반함수를 쓰고 .bind()로 원하는 객체를 지정하여 예측가능한 this로 사용하는 것이 좋다.

하지만 함수 안에 있는 함수 같은 경우에는 화살표 함수를 쓰는 것이 좋다.

객체 === this?

JS의 함수는 객체. 그 중에서도 일급 객체이다.

1. 변수나 데이터에 저장
   const myFunc = func
2. 함수의 인수로 전달
   function func1(func2) { }
3. 함수의 반환 값으로 사용
   function func1() {
   return func1
   }

이런 특징 때문에 자바스크립트 함수는 꽤나 다양한 환경에서 호출될 수 있다.

1. 단독으로 호출
   func()
2. 특정 개체에 메소드로 호출
   유정.func()
3. 동일한 함수가 다른 객체에 의해 호출
   감자.func()

자바스크립트에서는 this를 자기자신이라고 하기 까다롭다.
함수가 선언된 후 어떤 환경에서 어떤 객체에 의해 호출되었는지 예측할 수 없기 때문이다.

완벽하게 같은 함수라도 어떤 실행 환경에서 쉽게 말해 어떤 객체에 의해 호출되느냐에 따라 this의 의미가 달라진다.

자바스크립트에서 모든 함수는 this를 가지고 있다. 그리고 함수가 호출되면 그때 그때 상황에 따라 this가 가리키는 객체가 결정된다.

이렇게 함수가 호출될 때마다 this가 동적으로 결정되는 것을 ‘this가 그 객체에 **바인딩** 된다’ 라고 표현한다.

### 자바스크립트 엔진 - 실행 가능한 코드 - 실행 문맥

**자바스크립트 엔진**은 해당 프로그램을 실행하면 모든 **실행 가능한 코드**를 평가해서 **실행 문맥**이라는 것을 만든다.

실행 가능한 코드란 전역 코드, 함수 코드, eval 코드를 말하는데

각각의 실행 가능한 코드별로 실행 문맥이 하나씩 만들어진다.

이때 실행문맥은 렉시컬 환경 컴포넌트, 디스 바인딩 컴포넌트, …등 실제로 실행에 필요한 컴포넌트들로 이루어져 있는데

여기 이제 this바인딩 컴포넌트에 우리가 알고 있는 this에 대한 정보가 담기게 된다.

### 전역 실행 문맥 - (함수 실행) - 함수 실행 문맥

먼저 프로그램이 실행이 되면 자바스크립트 엔진은 전역코드를 평가에서 전역 실행 문맥을 만든다.

그리고 함수가 실행이 되면 전역 코드 실행을 잠깐 멈추고 또 다시 실행 문맥을 만들게 된다.

이 타이밍에서 this 바인딩 컴포넌트의 값이 결정된다.

### Object.Function()

일반적으로 this는 점 앞에 있는 객체. 즉 호출 당시 함수를 포함하고 있는 객체에 바인딩 된다.

### This Binding Rules

사실 각 실행 문맥에서 this를 바인딩하는 데에는 일정한 규칙들이 존재한다.

this는 기본적으로 네가지 규칙에 의해서 바인딩된다.

- 기본 바인딩
- 암시적 바인딩
- new 바인딩
- 명시적 바인딩
