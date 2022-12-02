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

### 46.4 제너레이터의 일시 중지와 재개

- 제너레이터는 `yield` 키워드와 `next()` 메서드를 통해 실행을 일시 중지했다가 필요한 시점에 다시 재개할 수 있다
- `yield` 는 함수 호출자에게 제어권을 양도할 때 사용하는 키워드이다
- 제너레이터의 이터레이션이 종료된 경우
    - `generatorResult.done` 에는 `true` 가 할당된다
    - `generatorResult.value` 에는 `undefined` 가 할당된다
        - 만약, 제너레이터 함수에 return 문이 존재한다면 return 한 값이 `generatorResult.value` 에 할당된다
- `generator.next()` 에는 인수를 전달할 수 있다!
- yield 한 값을 변수에 할당하는 경우, 다음 yield 가 실행될 때 할당된다

```jsx
function* generatorFunc() {
  /**
  * 첫 번째 next() 호출 시점에선 아직 const x 에는 값이 할당되지 않음
  * 두 번째 next() 를 호출할 때 해당 메서드에 인자로 전달한 값이 할당 됨
  * 
  * 즉, 두 번째 next() 가 호출될 때에는 10을 인자로 전달하기 때문에,
  * 해당 시점에 const x 에 10을 할당
  */
  const x = yield 1;

  /**
   *  
   */
  const y = yield (x + 10);

  return x + y;
}

const generator = generatorFunc();

/**
 * 처음 호출하는 next() 는 인자를 전달하지 않음
 * 만약 인자를 전달했다면 무시되며 첫 번째 yield 가 반환한 값이 result 객체에 할당됨
 */
let res = generator.next();
console.log(res);

/**
 * 이후 호출하는 next() 에는 인자를 전달할 수 있음
 * 이 때, 첫 번째로 선언한 yield 가 변수 선언문에 할당되어 있다면,
 * 이 시점에서 인자로 전달한 값이 첫 번째 yield 를 대입받는 변수에 할당됨
 * 
 * 즉, 이 시점에서 const x = 10 이 수행됨
 */
res = generator.next(10);
console.log(res);

res = generator.next(20);
console.log(res);
```

- 제너레이터의 특성을 이용하여 비동기 함수를 동기 방식처럼 동작하게할 수 있다

### 46.5 제너레이터의 활용

- 프로미스의 then/catch/finally 없이 비동기 처리 결과를 반환할 수 있지만 async/await 를 사용하면 이 짓을 안해도 된다

```jsx
// 함수 인자를 받는 익명 함수
const async = generatorFunc => {
  // 인자로 받은 함수를 generator 변수에 할당
  const generator = generatorFunc();

  // 재귀함수 생성
  // 실질적으로 async() 를 호출할 때 수행되는 함수
  const onResolved = arg => {
    // 이터레이션 수행
    const result = generator.next(arg);

    // n 회 반복하며 result.done 일 경우 result.value 를 반환
    // !result.done 인 경우 generator result 가 반환한 프로미스 객체를 통해
    // 비동기로 onResolved 를 재귀 호출
    return result.done
      ? result.value
      : result.value.then(res => onResolved(res));
  }

  return onResolved;
}

// fetchTodo() 라는 이름의 함수 객체를 생성하여 async() 의 인자로 전달
(async(function* fetchTodo() {
  const url = 'some url';

  // Caller 가 첫 번째로 next() 호출 시 GET {url} 호출
  const response = yield fetch(url);

  /**
   * Caller 가 두 번째로 .next() 호출 시
   * 
   * 1. fetch() 된 프로미스가 `const response` 에 할당
   * 2. `const response` 에 할당한 fetch response 객체의 json() 을 generator result 로 반환
   */
  // 이후, Caller 가 세 번째로 .next() 를 호출할 때 response.json() 이 `const todo` 에 할당되며 이터레이션 종료
  const todo = yield response.json();
  console.log(todo); // take response data from server
}))
```

- 제너레이터 실행기가 필요하다면…. 직접 구현하는 수고로움을 겪기 보다는 co 라는 라이브러리를 사용하면 된다고 한다..

### 46.6 async/await

- `async/await` 는 ECMAScript 2017(ES8) 에서 도입되었다고 한다
- `async/await` 는 프로미스를 기반으로 동작한다
    - `async/await` 를 사용하면 then/catch/finally 같은 후속 처리 메서드에 콜백 함수를 전달할 필요가 없다고 한다
    - 마치 동기 처리 방식처럼 프로미스를 사용할 수 있다
- `await` 키워드는 반드시 `async` 함수 내부에서 사용해야 한다
- `async` 함수는 언제나 프로미스를 반환한다
    - 명시적으로 프로미스를 반환하지 않더라도 프로미스로 래핑해서 반환해준다고 한다
- `await` 키워드는 프로미스가 settled 상태가 될 때 까지 대기하다가 settled 상태가 되면 resolve 한 처리 결과를 반환한다고 한다
    - 반드시 프로미스 앞에 선언해야 한다!
- `await` 을 통해 프로미스의 비동기 처리를 마치 동기 방식처럼 제어할 수 있게 되는 것이다
    
    ```jsx
    async function foo() {
      const a = await new Promise(resolve => setTimeout(() => resolve(1), 3000));
      const b = await new Promise(resolve => setTimeout(() => resolve(2), 2000));
      const c = await new Promise(resolve => setTimeout(() => resolve(3), 1000));
    
      // foo() 호출 후 실제로 console.log() 가 실행되기 까지 6초가 소요된다
      console.log([a, b, c]);
    }
    
    foo();
    ```
    
    - 이처럼 처리 순서가 보장되어야 하는 비동기 함수들은 `await` 키워드를 통해 제어할 수 있다
- `async/await` 는 try/catch/finally 를 사용할 수 있다!