## 18.1 일급 객체

- 일급 객체의 조건
    - 무명의 리터럴로 생성할 수 있다. 런타임에 생성이 가능하다.
    - 변수나 자료구조(객체, 배열 등)에 저장할 수 있다.
    - 함수의 매개변수에 전달할 수 있다.
    - 함수의 반환값으로 사용할 수 있다.
- 함수형 프로그래밍을 가능하게 하는 자바스크립트의 장점

## 18.2 함수 객체의 프로퍼티

- 함수도 프로퍼티를 가진다. `console.dir()`로 내부를 보면?

```jsx
function square(number) {
    return number * number;
}

console.dir(square);
// arguments: null
// caller: null
// length: 1
// name: "square"
// prototype: {constructor: f}
// [[FunctionLocation]]: VM298: 1
// [[Prototype]]: f ()
// [[Scopes]]: Scopes[1]
```

### 18.2.1 arguments 프로퍼티

- 함수 호출 시 전달된 인수의 정보를 담고 있다. `유사 배열 객체`
    - ES6 이후부터 유사 배열 객체이면서 이터러블이다.
- 함수의 매개변수보다 인수가 많을 때, 해당 프로퍼티에서 보관한다.
- 가변 인자 함수 구현시 유용할 것이다.

[이터러블, 이터레이터](https://www.notion.so/b22b7444063047c4ac23f70dffc8aee3) 

### 18.2.2 caller 프로퍼티

- ECMAScript에 포함되지 않은 비표준 프로퍼티

### 18.2.3 length 프로퍼티

- 함수 정의시 선언한 매개변수 개수

### 18.2.4 name 프로퍼티

- 함수 이름
- ES5까지는 무명함수의 경우 빈문자열
- ES6부터는 함수 객체를 가리키는 식별자를 이름을 가진다.

### 18.2.5 __proto__ 접근자 프로퍼티

- 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티
- `hasOwnProperty()` 는 객체 고유의 프로퍼티인지(true) 상속받은 프로퍼티인지(false)에 대한 결과를 반환한다.

### 18.2.6 prororype 프로퍼티

- constructor인 함수만 소유하는 프로퍼티
- 생성자 함수가 생성할 인스턴스의 프로토타입 객체