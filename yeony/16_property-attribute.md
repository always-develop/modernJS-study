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
    - `[[Value]]` : 값
    - `[[Writable]]` : true / false 변경 가능 여부
    - `[[Enumerable]]` : true / false 열거 가능 여부
    - `[[Configurable]]` : true / false 재정의 가능 여부

### 16.3.2 접근자 프로퍼티

- getter, setter 함수라고도 불린다.
- 값을 갖지 않고, 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 프로퍼티
    - `[[Get]]` : getter 함수 호출. 데이터 프로퍼티를 읽을 때 사용
    - `[[Set]]` : setter 함수 호출. 데이터 프로퍼티의 값을 저장할 때 사용
    - `[[Enumerable]]` : 데이터 프로퍼티와 동일한 의미
    - `[[Configurable]]` : 데이터 프로퍼티와 동일한 의미