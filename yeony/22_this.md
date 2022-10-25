# this

## 22.1 this 키워드

- 생성자 함수를 선언할 때, 인스턴스가 만들어지기도 전에 어떻게 인스턴스를 가리켜야할까?
- 자신이 속한 객체 또는 생성자 함수가 생성할 인스턴스를 가리키는 자기 참조 변수
    - 그렇기 때문에 자신이 속한 객체 또는 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.
- 엔진에 의해 암묵적으로 생성된다.
    - 함수 호출 방식에 따라 동적으로 결정된다.
- this는 키워드이지만 식별자 역할을 하며 this가 가리킬 객체를 바인딩한다.
- this는 어디서든 참조가 가능하다.

| this의 장소 | 의미 |
| --- | --- |
| 전역 | window |
| 일반 함수 | window |
| 객체의 메서드 | 호출한 객체 |
| 생성자 함수 | 인스턴스 |
| 일반 함수 (strict mode) | undefined |

## 22.2 함수 호출 방식과 this 바인딩

- 함수 호출 방식에 따라 this의 바인딩이 결정된다.
    - 일반 함수 호출
    - 메서드 호출
    - 생성자 함수 호출
    - Function.prototype.apply/call/bind 메서드에 의한 간접 호출

### 일반 함수 호출

- 중첩 함수이든, 콜백 함수이든 일반 함수에서 호출되면 this는 전역 객체이다.
- 객체 내부 중첩함수의 this를 객체 자신으로 주고 싶으면 따로 변수를 선언하여 사용하면 된다.
    - 또는 Function.prototype.bind() 사용
    - 또는 화살표 함수 사용

```jsx
var value = 1;

const thatObj = {
	value: 100,
	foo() {
		const that = this;
		
		setTimeout(function () {
			console.log(that.value); // 100
		}, 100);
	}
};

const bindObj = {
	value: 100,
	foo() {
		setTimeout(function () {
			console.log(this.value); // 100
		}.bind(this), 100);
	}
};

const arrowObj = {
	value: 100,
	foo() {
		setTimeout(() => console.log(this.value), 100); // 100
	}
};
```

### 22.2.2 메서드 호출

- 메서드를 호출한 객체에 바인딩 된다.
    - 다른 객체에 바인딩되면 해당 객체의 this
    - 일반 함수에 바인딩하면 전역이 this
- 프로토타입 메서드 내부에서 사용된 this 또한 호출한 객체에 바인딩된다.

### 22.2.3 생성자 함수 호출

- 미래의 인스턴스에게 바인딩된다.

### 22.2.4 Function.prototype.apply/call/bind 메서드에 의한 간접 호출

- 모든 함수가 상속받아 사용할 수 있다.

```jsx
function getThisBinding() {
	return this;
}

const thisArg = { a: 1 };

console.log(getThisBinding()); // window

// 인수로 전달한 객체를 this로 바인딩한다.
console.log(getThisBinding.apply(thisArg, [1, 2, 3])); // this는 {a: 1}
console.log(getThisBinding.call(thisArg, 1, 2, 3)); // this는 {a: 1}
/*
apply는 호출할 함수의 인수를 배열로 묶어 전달한다.
call은 호출할 함수의 인수를 쉼표로 구분된 리스트 형태로 전달한다.
두 메서드는 본질적으로 함수를 호출한다.
*/

// this의 바인딩을 교체하여 새로운 getThisBinding 함수를 생성해 반환한다.
console.log(getThisBinding.bind(thisArg)); // getThisBinding
// 함수를 호출하지는 않아서 따로 호출해야 한다.
console.log(getThisBinding.bind(thisArg)()); // {a: 1}
```

- 객체 내부 콜백 함수를 사용하려면 콜백 함수의 this와 객체 메서드의 this를 일치하게끔 작성해주어야 한다.
    - 콜백함수는 일반함수이기 때문에 this가 전역으로 잡힌다.