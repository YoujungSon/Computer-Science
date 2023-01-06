> **“JavaScript에서는 객체를 상속하기 위하여 프로토타입이라는 방식을 사용합니다.” - MDN**
> 

많은 글들이 prototype을 설명할 때 클래스의 상속이라는 단어를 섞어서 설명하는데 흔히들 클래스의 상속을 떠올리기 쉽다.

1. **prototype≠ 상속**

    엄밀히 말해서 prototype에서 상속은 클래스 상속과는 엄연히 다른 개념이다.

2. **자바스크립트에는 클래스가 없다.**
  
    왜냐하면 자바스크립트에서는 클래스가 없기 때문이다.

3. **자바스크립트에서 ‘복사’를 통한 상속은 없다.**
  
    그리고 내용을 복사해서 일어나는 상속 또한 자바스크립트 내에서는 존재하지 않기 때문이다.

4. **prototype은 클래스, 객체의 내용 복사 없이도 상속을 구현할 수 있게 해주는 방법이다.**
  
    prototype은 클래스나 객체 내용 복사 없이도 상속을 구현할 수 있게 해주는 하나의 방법이다.

5. **prototype은 ‘연결’이다.**
  
    prototype이 하는 일은 상속이 아니라 연결이다.

## **클래스가 없다면 객체를 어떻게 설계대로 찍어낼 수 있을까?**

- ES6 Class 문법이 그냥 Syntatic Sugar라는 의미는 아니다.

### **클래스: 객체를 찍어내는 틀**

클래스는 객체를 찍어내는 하나의 틀이라고 볼 수 있다. 자바 같은 언어에서는 클래스를 가지고 new 연산자와 함께 실행하면 그 틀의 형식에 맞춘 하나의 새로운 객체가 만들어진다.

```jsx
class Person {
  constructor(name) {
  this.name = name;
  }
  sayHello() {
    console.log(`${this.name}: hello!`);
  }
}
```

```jsx
function Person (name) {
  this.name = name;
  this.sayHello = function () {
    console.log(`${this.name}: hello!`);
  }
}
```

### **실제로 실행되는 코드는 클래스가 아니다.**

하지만 자바스크립트에서 말 그대로 이런 기능이 없다. 다만 함수를 이용해서 클래스 기능을 흉내낼 뿐이다. 위쪽의 클래스 문법은 사실상 아래쪽의 코드로 변환되어서 실행된 셈이다.

```jsx
function Person(name) {
  this.name = name;
  this.sayHello = function() {
    console.log(`${this.name}: hello!`);
  }
}

const chris = new Person('chris');
```

### **클래스가 아니라면 return이 없는데 객체가 어떻게 생성되는 걸까?**

Person함수는 return하는게 하나도 없는데 chris에는 객체가 하나 생성되어서 담기게 된다.
왜냐하면 함수와 new 연산자가 만나면 우리가 모르는 자바스크립트단에서 숨겨진 일들이 일어나기 때문이다.

1. new 연산자가 새로운 빈 객체를 메모리 상에 생성함
`const chris = new Person('chris');`
첫 번째로 new 연산자가 Person함수와 같이 있으면 새로운 빈 객체가 메모리상에 생성된다.
2. 생성된 빈 객체가 this에 바인딩 됨
    
    ```jsx
    function Person(name) {
      this.name = name;
      this.sayHello = function() {
        console.log(`${this.name}: hello!`);
      }
    }
    ```
    
    그 후에는 Person함수 안에 this에 생성된 빈 객체가 바인딩된다. 
    
3. this 객체의 속성을 채우는 동작이 수행됨
그 상태에서 Person 안의 내용이 실행되는데 그 내용이 this의 속성들을 할당하고 채워 주는 역할을 한다.
4. return 하는 것이 없다면 그렇게 만들어진 this가 return된다.
Person 함수에 return하는 게 아무것도 없다면 그렇게 만들어진 this가 최종적으로 return된다.

## **복사 없이 어떻게 상속을 수행할 수 있을까?**

```jsx
class Person {
  constructor(name) {
    this.name = name;
  }
  sayHello() {
    console.log(`${this.name}: hello`);
  }
}
class Crew extends Person {
  constructor(name) {
    super(name);
  }
  doCoding() {
    console.log(`${this.name}: coding...!`);
  }
}
```

### **Person + Crew 내용이 복사된 객체**

일반적인 클래스에서는 하나의 클래스가 부모클래스로부터 상속을 받게 되면 자식클래스로 이미 만들어진 인스턴스에는 부모와 자식 모두의 내용이 합쳐진 부모의 내용이 자식 클래스에 그대로 복사되어 들어간 내용이 인스턴스에도 반영되게 된다.

### **자바스크립트에서는 불가능**

만약에 상속이 복사를 의미하는 거라면 그런 의미에서 상속은 자바스크립트에서는 일어날 수 없다. 왜냐하면 자바스크립트는 객체 자체나 코드 내용을 복사하는 그런 깊은 복사를 수행하지 않는다. 복사할 수 있는 건 오로지 원시값과 객체의 참조값이다. 하지만 자바스크립트는 그래서 이 상속을 흉내내기 위해서 객체간의 연결이라는 개념을 활용한다.

### **__proto__ = 객체와 객체를 연결하는 링크**

연결은 Proto라는 이름의 속성을 바탕으로 수행된다.

자바스크립트의 모든 객체들은 proto라는 속성을 가지고 있는데 proto 속성은 객체와 객체 간의 연결하는 하나의 링크이다. 이런 객체와 객체 간의 링크 관계는 크게 3가지로 분리될 수 있다.

1. **다른 객체를 바탕으로 만들어진 객체**
객체는 자신의 원형이라고 할 수 있는 객체가 있다면 그 객체를 가리키는 __proto__ 링크를 자동으로 가짐
    
    ```jsx
    const newObj = Object.create(oldObj)
    newObj__proto__ === oldObj
    ```
    
    먼저 다른 객체를 바탕으로 만들어진 객체가 있는 경우이다. 이렇게 만들어진 객체는 자신의 원형이라고 할 수 있는 객체를 가리키는 proto 링크를 자동으로 자기만의 속성으로 가지게 된다.
    
2. **함수**
    
    ![image](https://user-images.githubusercontent.com/88040809/210955489-c8ffa51c-8fc9-46ed-b93d-31edff714aee.png)
    
    그냥 객체가 아니라 함수의 객체인 경우이다. 이 경우에는 proto 외에 함수의 prototype 객체를 만들어준다.
    예를 들어 Person 함수가 있다면 이 함수가 만들어질 때 Person과 자동으로 연결된 prototype 객체가 같이 만들어진다.
    Person 함수는 자신의 prototype 속성을 통해서 Person 함수의 prototype 객체를 가리키고 Person 함수의 prototype 객체는 prototype 객체 안에 constructor라는 속성을 통해서 Person 함수를 가리키는 순환참조 관계를 가지고 있다.
    
3. **new + 함수로 만들어진 객체**
`const chris = new Person('chris');` + alpha
**만들어진 새로운 객체에 __proto__ 링크가 Person 객체의 Prototype을 가리키게 됨**
new 연산자와 함수를 통해서 객체가 생성된 경우이다. 
생성된 객체에 자바스크립트가 생성자 함수의 prototype 객체를 가리키는 proto 링크를 그렇게 만들어진 객체 안에 넣는다.
**함수로 객체를 생성했을 때의 관계도**
    
    ![image](https://user-images.githubusercontent.com/88040809/210955504-bc1c5440-110c-427b-9467-b3212d1975b4.png)
    
    Person 함수에 의해 Kim 객체가 만들어졌다면 kim의 proto 링크는 Person 함수의 prototype 객체를 가리키게 된다.
    

# 프로토타입 체이닝

```jsx
function sayHello() {
  console.log(`${this.name}: hello`);
}

function Person(name) {
  this.name = name;
}

const chris = new Person('chris');

chris.sayHello();
```

chris 객체는 Person 함수에 의해서 만들어졌지만 chris  객체 안에는 sayHello라는 method가 없기 때문에 에러가 발생한다.

```jsx
function sayHello() {
  console.log(`${this.name}: hello`);
}

function Person(name) {
  this.name = name;
}

Person.prototype.sayHello = sayHello;

const chris = new Person('chris');

chris.sayHello();
```

Person 함수의 prototype 객체에 sayHello method를 하나 만들었을 뿐인데 chris 객체에서도 sayHello를 실행시킬 수 있게 된다.

왜냐하면 이 안에서는 prototype chaining이 일어나기 때문이다.

### **prototype chaining**

prototype chaining은 한마디로 간단히 말하자면 proto 링크를 따라가서 계속해서 탐색을 해나가는 과정 자체를 의미한다.

```jsx
const a = {
  attr1: 'moohan~',
};
const b = {
  attr2: 'mooyaho~',
};

a.__proto__ = b;

console.log(a.attr2); // 'mooyaho~'
```

![image](https://user-images.githubusercontent.com/88040809/210955548-8cf89475-9cad-43e8-b790-16bce283823c.png)

a 객체의 proto 링크를 직접적으로 b로 연결시키면 a 객체에는 없는 속성이 있어도 자바스크립트는 proto 링크를 통해 b 객체를 이동하고 거기서 또 속성을 찾아보기 때문에 a 속성에 없는 속성도 b 속성에 있다면 a 객체에서 자신에게 없는 속성도 사용할 수 있게 된다. 
이런 식으로 proto 링크를 따라서 거슬러 올라가서 탐색을 수행하는 것이 바로 prototype chaining이다.

```jsx
function sayHello() {
  console.log(`${this.name}: hello`);
}

function Person(name) {
  this.name = name;
}

const chris = new Person('chris');

chris.sayHello();
```

### **chris.sayHello**

이 한줄의 레코드는 어떻게 그냥 단순하게 보면 chris라는 객체에 직속된 sayHello method를 접근하려는 코드처럼 보이지만 내부에서는 좀 더 많은 일이 일어난다.

![image](https://user-images.githubusercontent.com/88040809/210955593-354ce511-aebf-4847-9bfc-49f978eeb2a8.png)

sayHello를 찾을 때까지 
__proto__ 링크를 
거슬러 올라감

먼저 저 코드는 chris라는 객체에 sayHello 속성이 있는지 먼저 찾아봅니다.
당연히 찾을 수가 없겠죠 그러면 거기서 멈추는 게 아니라 chris 객체 안에  proto 속성을 통해서 chris 객체를 생성했던 Person 함수의 prototype 객체로 이동해서 다시 한번 sayHello를 찾아간다.

이때 여기서 sayHello를 찾아낼 수 있기 때문에 chris객체 내부에 sayHello가 있든말든 sayHello를 실행시킬 수 있게 된다. 

하지만 Person prototype 객체 안에서도 sayHello를 찾지 못한 경우라면 이번에는 Person prototype 객체의 proto 링크를 타고 또 올라가게 된다.

![image](https://user-images.githubusercontent.com/88040809/210955630-3a2879a1-ac22-4579-ad6b-273c6ec2d5a8.png)

그런데 이 proto 링크는 Person 이라는 함수를 생성한 생성자에 prototype 객체로 이동하게 되고 여기서도 sayHello를 찾아보는 그래도 발견 못하면 이런 식의 연쇄를 계속 하게 된다.

### **chris.sayHello ⇒ undefined**

![image](https://user-images.githubusercontent.com/88040809/210955663-81fb5605-81bf-4565-94b8-0ef42f9445ea.png)

이 연쇄는 오브젝트라는 이름의 생성자 함수에 prototype 객체에 도달했을 때 겨우 멈춘다.
이 때는 proto 링크 안에 null이 있어서 더 이상 prototype chaning을 지속 시킬 수 없기 때문이다.

그래서 결국 sayHello를 찾지 못 하게 되면 undefined를 반환하게 된다.

### **할당할 때는 어떤 일이 일어날까?**

sayHello method를 새로 할당하는 경우는 어떻게 될까? 

```jsx
function sayHello() {
  console.log(`${this.name}: hello`);
}

function Person(name) {
  this.name = name;
}

const chris = new Person('chris');

chris.sayHello = function () {
  console.log('hi~!');
};
```

```jsx
// Object.defineProperty( 대상 객체, '프로퍼티 key', {프로퍼티 디스크립터 객체})
// 프로퍼티의 어트리뷰트를 정의

Object.defineProterty(Person.prototype. ‘sayHello’.{

  writable: false //해당 속성의 값을 변경할 수 있는지 여부

  …

})

// 엄격모드 : 에러

// 비엄격모드: 아무 일 없음
```

첫 번째 경우에는 prototype chaining 과정을 거쳐서 도달한 sayHello method가 읽기 전용이라면 일단, 자바스크립트가 엄격 모드라면 에러가 발생한다. 하지만 비엄격 모드에서는 아무 일도 일어나지 않는다. 

**Person.prototype.sayHello가 setter일 경우 그냥 setter를 실행**

sayHello method가 그냥 setter라면 그냥 그 setter가 수행된다. 

```jsx
object.definProperty(Person.prototype. ‘sayHello’. {

  writable: true
  …

})
// chris.sayHello 추가
```

하지만 sayHello method가 읽기전용이 아니라면 Person prototype 객체의 sayHello method에 무엇인가가 덮어 씌워지는 것이 아니라 chris 객체의 sayHello method가 추가되게 된다.

```jsx
function sayHello() {
  console.log(`${this.name}: hello`);
}

function Person(name) {
  this.name = name;
}

Person.prototype.sayHello = sayHello;

const chris = new Person('chris');

chris.sayHello = function () {
  console.log('chris: hi~!');
};

chris.sayHello(); // chris: hi~!
```

1. **chris 객체에서 이 함수에 정상적인 방법으로는 접근 불가**

마지막 경우에는 chris 객체를 통해서 person prototype의 sayHello에 접근할 수 있는 방법이 사라져 버렸다.

왜냐하면 chris 객체 안에서 sayHello method를 찾을 수가 있기 때문에 prototype chaining이 거기서 멈춰 버릴 것이기 때문이다. 이런 식으로 같은 이름의 속성을 객체 안에 넣어버리면 원래의 prototype 객체의 속성에는 접근할 수 없게 되는 경우가 있는데 이를 가려짐이라 부른다.

1. **Method Overriding X 가려짐 O**

자바스크립트에서는 class method overriding 대신 가려짐이라는 것이 있는 것이다.
