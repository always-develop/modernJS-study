- 클래스, 상속, 캡슐화를 위한 키워드가 없어서 객체 지향 언어가 아니다? (public, private, protected 등)
- 프로토타입 기반의 객체 지향 프로그래밍이다!

## 19.1 객체지향 프로그래밍

- 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임
- 어떤 사물이나 개념을 인식하는 철학적 사고를 프로그래밍에 접목하여 그 실체는 속성을 가지고 있다고 말한다.

> **추상화?**
다양한 실체의 속성 중 필요한 것만 표현하는 것
> 
- 객체
    - 속성을 통해 여러 개의 값을 하나의 단위로 구성한 복합적인 자료구조
    - 상태 데이터와 동작을 하나의 논리적인 단위로 묶은 복합적인 자료구조
        - 상태 데이터: 프로퍼티
        - 동작: 메서드
    - 다른 객체와 관계성을 맺을 수도 있다.
        - 메시지 주고받기
        - 데이터 처리
        - 상속

## 19.2 상속과 프로토타입

- 객체지향 프로그래밍의 핵심..!
- 자바스크립트는 프로토타입을 기반으로 상속을 구현해 불필요한 중복을 제거한다.
- 아래의 생성자 함수를 보자.

```jsx
// 지름이 다른 원 인스턴스를 생성하는 생성자 함수
function Circle(radius) {
	this.radius = radius;
	this.getArea = function() {
		return Math.PI * this.radius ** 2;
	};
}

const circle = new Circle(2);
const circle2 = new Circle(2);

console.log(circle.getArea() === circle2.getArea()); // true
console.log(circle.getArea === circle2.getArea); // false
// 같은 값을 도출하지만 각각의 함수는 다른 장소에 저장되어 있다.
```

- 위의 생성자 함수로 생성된 인스턴스들은 본인들의 `radius`  속성과 `getArea()` 메서드를 가진다.
    - 함수 객체들이 다른 메모리에 저장된다는 뜻이다.
- `상속` 을 사용하면 이러한 문제가 해결된다.

```jsx
function Circle(raiuds) {
	this.radius = radius;
}

// Circle의 프로토타입에 상속해줄 값을 추가한다
Circle.prototype.getArea = function() {
	return Math.PI * this.radius ** 2;
};

const circle = new Circle(1);
const circle2 = new Circle(2);

console.log(circle.getArea === circle2.getArea); // true
```

- 위의 방식으로 객체의 프로토타입에 상속 시켜줄 프로퍼티나 메서드를 추가하면 해당 객체의 인스턴스들은 개인의 프로퍼티만 보관하고 있고, 상속 받은 프로퍼티나 메서드는 부모의 것을 참조한다.
    - 이는 메모리를 절약하는 방법이 되고, 코드 재사용도 가능하다.

## 19.3 프로토타입 객체

- 객체 간의 상속을 구현하기 위해 사용된다.
- 모든 객체는 `[[Prototype]]` 이라는 내부 슬롯을 가지고 있다.
    - 여기에 저장되는 프로토타입은 객체 생성 방식에 의해 결정된다.
    - `[[Prototype]]` 값이 null인 객체는 없다.

### 19.3.1 __proto__ 접근자 프로퍼티

- 요것으로 `[[Prototype]]` 내부 슬롯에 간접 접근이 가능하다.

**__proto__는 접근자 프로퍼티다.**

- `Value` 가 없고, `getter`과 `setter` 를 가지고 있다.
    - `getter`과 `setter`를 통해 `[[Prototype]]` 에 있는 값을 가져오거나 할당한다.

**__proto__ 접근자 프로퍼티는 상속을 통해 사용된다.**

- 이것은 `Object.prototype`의 프로퍼티다.
- ~~모든 객체의 최상위 부모는 `Object` 이다.~~

**__proto__ 접근자 프로퍼티를 통해 프로토타입에 접근하는 이유**

- 서로가 자신의 프로토타입이 되는 것을 방지하기 위함..!
    - 다른 말로 프로토타입 체인은 단방향 상속으로 이루어지는데 양방향을 막기 위함!

**__proto__ 접근자 프로퍼티를 코드 내에서 직접 사용하는 것은 권장하지 않는다.**

- 모든 객체가 `__proto__` 를 상속받는 것은 아니다.
    - 직접 상속을 통해 `Object.prototype`을 상속받지 않는 객체를 생성할 수도 있다.

```jsx
// prototype이 null인 경우
const obj = Object.create(null);

// Object.prototype을 상속받은 것이 아니라서 __proto__ 사용이 안되기 때문에 아래 메서드 이용
console.log(Object.getPrototypeOf(obj)); // null
```

- 위의 경우 프로토타입 재할당을 원하면 `setPrototype(타겟 객체, 바꾸고 싶은 프로토타입의 객체)` 메서드를 이용한다.

### 19.3.2 함수 객체의 prototype 프로퍼티

- 함수 객체만 소유하는 prototype 프로퍼티가 있다.
    - `[[Construct]]` 내부 메서드를 가진 함수만 가지고 있다.
- `Object.prototype`의 `__proto__` 접근자 프로퍼티와 함수 객체의 `prototype` 프로퍼티는 동일한 프로토타입을 가리킨다.
    - 사용 주체와 목적이 다르다.

```jsx
function Person(name) {
	this.name = name;
}

const me = new Person('Kim');

console.log(Person.prototype === me.__proto__); // true
```

### 19.3.3 프로토타입의 constructor 프로퍼티와 생성자 함수

- `constructor`를 가진 생성자 함수에서 나온 인스턴스는  `constructor` 프로퍼티를 가지고 있다.
    - 프로퍼티를 가지고 있는 것이지 생성자 함수가 가지고 있는 `[[Construct]]` 내부 메서드를 가지고 있는 것이 아니다.

```jsx
function Person(name) {
	this.name = name;
}

const me = new Person('Kim');

console.log(me.constructor === Person); // true
console.log(me.constructor);
/*
	function Person(name) {
		this.name = name;
	}
*/

new me; // Uncaught TypeError: me is not a constructor
```

## 19.4 리터럴 표기법에 의해 생성된 객체의 생성자 함수 프로토타입

- 리터럴로 생성된 객체와 생성자 함수로 생성된 객체는 완전하게 동일할까?

```jsx
const useConstructorObj = new Object();

const useLiteralObj = {};

console.log(useConstructorObj.constructor === useLiteralObj.constructor); // true
```

- 일단 두 객체 생성 방식의 `constructor`는 동일한 것을 알 수 있다.
- ECMAScript의 생성자 함수의 구현법은 아래와 같다.

> **Object([value])**
When the Object function is called with optional argument value, the following steps are taken:
1. If NewTarget is neither undefined nor the active function, then
    a. Return? OrdinaryCreateFromConstructor(NewTarget, “%Object.prototype%”).
2. If value is undefined or null, return OrdinaryObjectCreate(”%Object.prototype%”).
3. Return ! ToObject(value).
…
> 

> **Object([value])**
인수의 유동성이 있는 객체 함수가 호출될 때, 다음과 같은 단계를 따릅니다.
1. 만약 NewTarget이 undefined도 아니고, active function도 아닐 때는 OrdinaryCreateFromConstructor()를 호출하여 반환합니다.
2. 만약 value가 undefined거나 null일 때는 OrdinaryObjectCreate()를 호출하여 반환합니다.
3. value가 있으면 ToObject(value)를 반환합니다.
…
> 

- ECMAScript의 리터럴 객체의 구현법은 아래와 같다.

> **ObjectLiteral: {}**
1. Let obj be Return OrdinaryObjectCreate(”%Object.prototype%”).
2. Perform ? PropertyDefinitionEvaluation of PropertyDefinitionList with arguments obj and true.
3. Return obj.
> 

> **ObjectLiteral: {}**
1. OrdinaryObjectCreate()로 객체를 반환합니다.
2. 인수와 함께 PropertyDefinitionList의 프로퍼티 정의 평가를 수행합니다. (?)
3. 객체를 반환합니다.
> 
- 두 방식을 비교해보면 리터럴로 생성된 객체는 처음 new.target의 확인이나 프로퍼티를 추가하는 처리 내용 등 세부 내용은 다르다.
    - 따라서 완전 동일하지 않다.

- 함수의 경우 더 명확하게 다르다.
- Function 를 사용한 함수는 렉시컬 스코프를 만들지 않고, 전역 함수인 것처럼 스코프를 생성하며 클로저도 만들지 않는다.

```jsx
function foo() {}

// 하지만 요렇게 비교해보면 foo의 생성자 함수는 Function의 생성자 함수다.
console.log(foo.constructor === Function); // true
```

- 프로토타입은 생성자 함수와 함께 생성되며 prototype, constructor 프로퍼티에 의해 연결되어 있다.
- 결론적으로 리터럴표기법과 생성자함수로 생성된 객체는 미묘한 차이는 있겠으나 본질적으로 큰 차이는 없기 때문에 사용하는데 큰 무리는 없다.

## 19.5 프로토타입의 생성 시점

- 프로토타입은 생성자 함수가 생성되는 시점에 생성된다.

### 19.5.1 사용자 정의 생성자 함수와 프로토타입 생성 시점

- 생성자 함수로 호출할 수 있는 시점에 프로토타입이 생성된다.
    - 당연한 말이지만 non-constructor은 프로토타입이 없다.

### 19.5.2 빌트인 생성자 함수와 프로토타입 생성 시점

- 빌트인 생성자 함수가 생성되는 시점에 프로토타입이 생성된다.
    - 빌트인 생성자 함수는 전역 객체가 생성되는 시점에 생성되기 때문에 사용자 객체가 생성되기 이전에 이미 존재한다.

## 19.6 객체 생성 방식과 프로토타입의 결정

- 객체를 생성하는 방식은 여러 가지이지만 모두 OrdinaryObjectCreate에 의해 생성된다.

### 19.6.1 객체 리터럴에 의해 생성된 객체의 프로토타입

- 객체 리터럴에 의해 생성된 객체의 프로토타입은 Object.prototype이다.
- 가상의 Object 생성자 함수가 존재하고, Object.prototype에게 상속 받는다.
- 리터럴 내부에서 프로퍼티 추가

### 19.6.2 Object 생성자 함수에 의해 생성된 객체의 프로토타입

- 인수 없이 Object 생성자 함수로 객체를 생성하면 빈 객체가 생성되며 이 때 OrdinaryObjectCreate가 호출된다.
- Object 생성자 함수에게 Object.prototype 을 상속 받는다.
- 빈 객체 생성 후 프로퍼티 추가

### 19.6.3 생성자 함수에 의해 생성된 객체의 프로토타입

- 사용자 정의 생성자 함수에게 Object.prototype 을 상속 받는다.
- 그 외 사용자 정의 생성자 함수에게 새로운 프로토타입을 정의할 수 있고, 그 객체의 인스턴스들은 사용자 정의 생성자 함수의 프로토타입을 상속받는다.

## 19.7 프로토타입 체인

- 객체의 프로퍼티 또는 메서드에 접근하려 할 때, 해당 객체에서 찾을 수 없다면 `[[Prototype]]`내부 슬롯의 참조를 따라 객체의 부모 단으로 순차적 탐색을 시작한다.
- 예시는 아래와 같다.

```jsx
function Person(name) {
	this.name = name;
}

const me = new Person('Kim');

me.hasOwnProperty('name');

/*
me에서 hasOwnProperty 메서드를 탐색
 -> Person에서 hasOwnProperty 메서드를 탐색
 -> Object.prototype에서 hasOwnProperty 메서드를 탐색
 -> Object.prototype에서 hasOwnProperty 메서드 호출
    Object.prototype.hasOwnProperty.call(me, 'name');
*/
```

- Object 의 프로토타입 체인 종점은 `Object.prototype` 이다.
    - 종점에서 찾을 수 없는 프로퍼티나 메서드라면 `undefined`를 반환하며 에러는 발생하지 않는다.

**스코프 체인과 프로토타입 체인은 서로 협력하여 식별자와 프로퍼티를 검색하는데 사용된다.**

- 스코프 체인
    - 식별자 검색을 위한 메커니즘
- 프로토타입 체인
    - 상속과 프로퍼티 검색을 위한 메커니즘

## 19.8 오버라이딩과 프로퍼티 섀도잉

- `프로토타입 프로퍼티/메서드` vs. `인스턴스 프로퍼티/메서드`
    - 두 개의 이름이 같은 이름으로 생성된다면 어떻게 될까?
        - 인스턴스의 프로퍼티가 프로토타입의 프로퍼티를 오버라이딩하여 `프로퍼티 섀도잉` 현상이 발생한다.
    - 같은 이름의 프로퍼티를 인스턴스 프로퍼티에서 삭제해버린다면?
        - 인스턴스 프로퍼티가 삭제되고 프로토타입의 프로퍼티는 존재한다.
    - 인스턴스 접근으로 프로토타입의 프로퍼티를 삭제할 수 있을까?
        - 하위 객체에서 `get` 접근은 가능하나 `set` 접근은 허용되지 않아서 삭제되지 않는다.
        - 프로퍼티 삭제는 해당 프로퍼티를 소유하고 있는 객체에 접근하여 삭제해야 한다.

## 19.9 프로토타입의 교체

### 19.9.1 생성자 함수에 의한 프로토타입의 교체

```jsx
const Person = (function () {
	function Person(name) {
		this.name = name;
	}

	return Person;
}());

console.log(Person.prototype); // {constructor: ƒ}

Person.prototype = {
	sayHello() {
		console.log(`Hi! My name is ${this.name}`);
	}
};

// 기존의 constructor 프로퍼티가 없어지고 새로운 프로퍼티로 교체된다.
// constructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다.
console.log(Person.prototype); // {sayHello: ƒ}

const me = new Person('Kim');

// me의 constructor는 Person에서 가져온 것이 아니고, Object의 것으로 교체된다.
console.log(me.constructor); // Object()
```

- 위의 방법으로 사용자 정의 생성자 함수의 프로토타입을 교체할 수도 있고 constructor 프로퍼티를 추가하여 되살릴 수도 있다.

### 19.9.2 인스턴스에 의한 프로토타입의 교체

- 이미 생성된 객체의 프로토타입을 교체하는 것이다.

```jsx
function Person(name) {
  this.name = name;
}

const me = new Person('Kim');

// 기존 프로토타입
Object.getPrototypeOf(me); // {constructor: ƒ}

const parent = {
  sayHello() {
      console.log(`Hi, my name is ${this.name}`);
  }
};

// 프로토타입 교체
Object.setPrototypeOf(me, parent);

// 교체된 프로토타입
Object.getPrototypeOf(me); // {sayHello: ƒ}
```

- 이것 또한 me의 constructor은 Object와 연결된다.
    - constructor를 소유하던 Person과 연결이 파괴되었기 때문이다.
    - 또한 임의로 되살리기도 가능하다.
- 하지만 프로토타입 상속 관계를 조작하는 것은 안전하지 않다.

## 19.10 instanceof 연산자

- 문법
    - `{객체} instanceof {생성자 함수}`
- 우변의 생성자 함수가 좌변의 객체의 프로토타입을 소유하고 있다면 true, 아니면 false 평가
- 프로토타입 체인을 연결되어 있는가를 평가할 수 있다.
- 19.9에 나온 내용처럼 생성자 함수의 프로토타입을 교체하여도 생성자 함수와 constructor 프로터피의 연결이 파괴되는 것이지 생성자함수의 prototype 프로퍼티와 프로토타입 간의 연결은 파괴되지 않았으므로 `instanceof`에 영향을 주지 않는다.

## 19.11 직접 상속

### 19.11.1 Object.create에 의한 직접 상속

- 이 친구도 또한 OrdinaryObjectCreate를 호출한다.
    - 첫번째 매개변수에는 생성할 객체의 프로토타입 객체
    - 두번째 매개변수는 생성할 객체의 프로퍼티 키와 프로퍼티 디스크립터 객체
        - Object.defineProperties()의 인수와 동일하게 작성한다.
- 인수가 `null` 이면 Object.prototype 상속을 받지 못한다.
- 객체를 생성하며 직접 상속을 구현하는 방식이다.