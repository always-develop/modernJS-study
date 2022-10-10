## 16.1 내부 슬롯과 내부 메서드

- ECMAscript 사양에서 사용하는 의사 프로퍼티와 의사 메서드
- 직접 접근은 불가
    - 객체의 경우  `__proto__` 를 통해 프로토타입에 간접 접근 가능

```jsx
const o = {};

o.[[Prototype]] // SyntaxError

o.__proto__ // Object.prototype

console.log(o); // {} 하위 항목 [[Prototype]] 확인 가능
```

## 16.2 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체

- 프로퍼티 생성시 프로퍼티 어트리뷰트를 기본값으로 자동 정의한다.
    - 프로퍼티의 상태를 나타낸다.
        - 프로퍼티의 값, 값 갱신 가능 여부, 열거 가능 여부, 재정의 여부
- 엔진이 관리하는 내부 상태 값인 내부 슬롯이다.
    - [[Value]], [[Writable]] .. 등
    - 간접 접근 가능 : `Object.getOwnPropertyDescriptor`

```jsx
const person = {
	name: 'Lee'
};

console.log(Object.getOwnPropertyDescriptor(person, 'name'));
// 첫번째 매개변수 : 객체의 참조
// 두번째 매개변수 : 프로퍼티 키(String)
// 프로퍼티 디스크립터 객체 반환 {value: 'Lee', writable: true, enumerable: true, configurable: true}
```

- 존재하지 않는 프로퍼티나 상속받은 프로퍼티를 요구시 `undefined` 반환
- ES8 : `Object.getOwnPropertyDescriptors`
    - 매개변수로 객체 참조만 전달하면 모든 프로퍼티 키에 대한 상태를 반환한다.

## 16.3 데이터 프로퍼티와 접근자 프로퍼티

### 16.3.1 데이터 프로퍼티

- 일반적으로 키와 값으로 구성된 것. 우리가 알고 있는 형태
    - `[[Value]]` : 값, default **undefined**
    - `[[Writable]]` : default **false** / true 변경 가능 여부
    - `[[Enumerable]]` : default **false** / true 열거 가능 여부
    - `[[Configurable]]` : default **false** / true 재정의 가능 여부

### 16.3.2 접근자 프로퍼티

- getter, setter 함수라고도 불린다.
- 값을 갖지 않고, 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 프로퍼티
    - `[[Get]]` : getter 함수 호출. 데이터 프로퍼티를 읽을 때 사용, default **undefined**
    - `[[Set]]` : setter 함수 호출. 데이터 프로퍼티의 값을 저장할 때 사용, default **undefined**
    - `[[Enumerable]]` : 데이터 프로퍼티와 동일한 의미
    - `[[Configurable]]` : 데이터 프로퍼티와 동일한 의미

```jsx
const person = {
 // firstName, lastName: 데이터 프로퍼티
	firstName: 'Ungmo',
	lastName: 'Lee',
	
  // fullName(): 접근자 프로퍼티
	// getter 함수
	get fullName() {
		return `${this.firstName} `${this.lastName}`
	},
	
	// setter 함수
	set fullName(name) {
		return [this.firstName, this.lastName] = name.split(' ');
	}
};
```

> **프로토타입?**
어떤 객체의 상위 객체의 역할을 하며 하위 객체에게 자신의 프로퍼티와 메서드를 상속한다. 역은 성립하지 않는다.
  → 단방향 링크드 리스트 형태이며 프로토타입 체인이라고 한다. 
어떤 객체의 프로퍼티나 메서드에 접근하려할때, 해당 객체에서 찾을 수 없으면 프로토타입 체인에 따라 탐색해나간다.
> 

## 16.4 프로퍼티 정의

- 새로운 프로퍼티를 추가하거나 기존 프로퍼티 어트리뷰트를 재정의하는 것
- `Object.defineProperty()` : 새로운 프로퍼티 정의 가능
- `Object.definePropertis()` : 여러 개의 프로퍼티 정의 가능

```jsx
const person = {};

// 디스크립터 객체의 모든 프로퍼티를 정의
Object.defineProperty(person, 'firstName', {
	value: 'Ungmo',
	writable: true,
	enumerable: true,
	configurable: true
});

// 위의 방식과 동일한 결과를 보여줌
person.firstName = 'Ungmo';

// 디스크립터 객체의 누락된 프로퍼티가 있으면 'false' 정의됨
Object.defineProperty(person, 'lastName', {
	value: 'Lee'
});
```

- `writable: false`
    - 해당 프로퍼티 변경시 에러 미발생하며 수정되지 않음
- `enumerable: false`
    - 객체 키 반복문 실행시 해당 프로퍼티가 계산되지 않음
- `configurable: false`
    - 해당 프로퍼티 삭제 시도시 에러 미발생하며 삭제되지 않음

## 16.5 객체 변경 방지

- 객체는 변경이 가능한 값이기 때문에 이를 의도적으로 방지하기 위한 메서드들이 존재한다.
- 금지 기능이 부여된 객체에 금지 기능을 사용하면 적용이 되지 않을 뿐, 에러는 발생하지 않는다.
    - strict mode 에서는 에러 발생

### 16.5.1 객체 확장 금지 `Object.preventExtensions()`

- 객체 프로퍼티 추가 금지
    - `Object.isExtensible()` 로 확인 가능

### 16.5.2 객체 밀봉 `Object.seal()`

- 객체 프로퍼티 추가, 삭제 금지
    - `Object.isSealed()` 로 확인 가능

### 16.5.3 객체 동결 `Object.freeze()`

- 읽기 전용 객체
    - `Object.isFrozen()` 로 확인 가능

### 16.5.4 불변 객체

- 위의 방식들은 얕은 변경 방지만 된다.
    - 객체 내부의 객체에는 영향을 주지 못한다.
- `Object.freeze` 메서드를 활용해 모든 프로퍼티를 동결시켜버리는 함수를 만들어 써야한다.