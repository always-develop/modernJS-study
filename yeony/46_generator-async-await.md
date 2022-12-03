## 46.1 제너레이터란?

- 코드 블록의 실행을 일시 중지했다가 재생할 수 있는 함수
- 특징
    - 호출자에게 함수 실행의 제어권을 양도할 수 있다.
    - 함수와 호출자와 양방향으로 함수의 상태를 주고 받을 수 있다.
    - 함수 코드를 실행하는 것이 아니라 이터러블이면서 이터레이터인 제너레이터 객체를 반환한다.

## 46.2 제너레이터 함수의 정의

- `function*` 키워드로 선언한다.
- `yield` 표현식을 포함한다.
- 화살표 함수로는 정의할 수 없다.
- 생성자 함수로 호출할 수 없다.

## 46.3 제너레이터 객체

- 제너레이터 함수가 반환한 제너레이터 객체는 이터러블이면서 이터레이터다.
    - `value`, `done` , `next()` 를 소유한다.
    - `return` , `throw` 도 가진다.

```jsx
function* genFunc() {
	try {
		yield 1;
		yield 2;
		yield 3;
	} catch (e) {
		console.error(e);
	}
}

const generator = genFunc();

/**
next(): 다음 yield 값으로 value를 뱉는다.
return('VALUE'): return 인수로 value를 뱉는다.
throw('ERROR'): throw 인수로 value는 undefined를 뱉는다.
**/
console.log(generator.next()); // {value: 1, done: false}
console.log(generator.return('End!')); // {value: 'End!', done: true}
```

## 46.4 제너레이터의 일시 중지와 재개

- next()로 제너레이터 함수를 제어한다.
- yield 표현식을 순서대로 실행한다.
- done의 값으로 끝인지 아닌지를 알 수 있다.
- next() 에 인수를 가질 수 있는 특징을 가지고 있다.
    - next의 인수는 yield 표현식에 할당 받는 변수에 할당된다.

## 46.6 async, await

- 제너레이터보다 가독성이 좋은 비동기 처리 방법
- 프로미스 기반 동작

### async

- `await` 는 `async` 내부에서만 사용해야 한다.
- `async` 가 사용한 함수는 항상 프로미스를 반환한다.
- 클래스의 constructor 메서드는 async 메서드가 될 수 없다.

### await

- 프로미스가 `settled` 상태가 될 때까지 대기하다가 되면! 프로미스가 resolve한 처리 결과를 반환한다.
- 모든 프로미스에 await를 사용하지 말자.
    - 순차적인 처리가 필요할 때만 사용하자.

### 에러 처리

- `async` , `await` 는 try…catch 문을 사용하여 에러 처리가 가능하다.
