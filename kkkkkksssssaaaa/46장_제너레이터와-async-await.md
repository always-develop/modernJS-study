# 46장. 제너레이터와 async/await

# [javascript] ☕ 모던 자바스크립트

---

## 46장. 제너레이터와 async/await

### 46.1 제너레이터란?

- 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수라고 한다
- 일반 함수와는 아래와 같은 차이가 있다
    - 제너레이터 함수는 함수 호출자에게 함수 실행 제어권을 양도할 수 있다
        - 즉, 실행될 함수(`Callee`)만 제어권을 갖는 것이 아닌, 호출한(`Caller`) 함수에게 제어권을 양도(`yield`)할 수 있다는 것을 의미
    - 제너레이터 함수는 함수 호출자와 함수 상태를 주고받을 수 있다
        - 즉, Caller 에게 Callee 의 상태를 양방향으루 주고받을 수 있다
    - 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다
        - 즉, 단순하게 함수 코드를 실행하는 것이 아니라, 이터러블이면서 동시에 이터레이터인 제너레이터 객체를 반환

### 46.2 제너레이터 함수의 정의

- 제너레이터 함수는 다음과 같이 정의할 수 있다

```jsx
function* genDecFunc() {
  yield 1;
}

const genExpFunc = function* () {
  yield 1;
}

const obj = {
  * genObjMethod() {
    yield 1;
  }
}

class MyClass {
  * genClsMethod() {
    yield 1;
  }
}
```

- 제너레이터는 화살표 함수로 정의할 수 없다
- 제너레이터는 생성자 함수로 호출할 수 없다

### 46.3 제너레이터 객체

- 제너레이터 함수를 호출하면 코드 블록을 실행하는 것이 아니라 제너레이터 객체를 생성하여 반환한다
    - 제너레이터 객체는 이터러블이면서 동시에 이터레이터이다
    
    ```jsx
    function* genFunc() {
      yield 1;
      yield 2;
      yield 3;
    }
    
    const generator = genFunc();
    
    console.log(Symbol.iterator in generator); // true
    console.log('next' in generator); // true
    ```
    
- 제너레이터 객체는 이터레이션이 종료됐는지를 판단하는 `done` 이라는 프로퍼티를 가지고 있다
    - 제너레이터 객체의 `return()` 메서드를 호출하면 이터레이션이 종료되며 `done` 프로퍼티의 값이 `true` 로 설정된다
- 제너레이터의 이터레이션이 종료되면 `return()` 된 객체를 가지고 있는 Iterator result 객체를 반환한다