## 11.1 원시 값

---

### 11.1.1 변경 불가능한 값

- 변경 불가능한 값
    - 원시 값 자체를 변경하지 못한다는 말
    - 원시 값을 변경 불가능하다는 말은 메모리 관점에서 접근해야 이해가 쉽다.
    - 원시 값은 변경 불가능하기 때문에 변수에 값 재할당 시 기존 메모리 공간이 아닌 새로운 메모리 공간을 확보하여 저장하는 것이다.
- 변수의 값을 변경 불가능한 것은 상수이다. → 재할당 금지!

### 11.1.2 문자열과 불변성

- 문자열 타입 - 2바이트
    - 문자 하나당 2바이트, 문자의 개수에 따라 확보하는 메모리 공간이 달라짐
- 숫자 타입 - 8바이트
    - 숫자의 크기에 따라 관계없이 8바이트 사용

<aside>
❓ bigInt의 경우?

</aside>

- 이외 원시 타입은 브라우저 제조사의 구현에 따라 크기가 다를 수 있다.
- 자바스크립트는 문자열의 형태로 데이터를 저장한다.
    - 문자열 타입의 변수에 다른 문자열을 재할당하면 새로운 메모리 공간에 저장한다.
    - 문자열은 문자의 배열이기 때문에 `index` 를 사용하여 하나의 문자에 접근할 수 있다.
    - 접근은 가능하다.  그러나 이미 원시값 형태로 만들어진 문자열이기 때문에 해당 인덱스에 접근하여 값을 할당하는 시도를 하더라도 값은 변하지 않는다. 에러는 발생하지 않는다.

### 11.1.3 값에 의한 전달

- 값을 가진 변수를 다른 변수에 할당을 한다면?
    - ECMAScript 사양에 변수를 통한 메모리 관리에 관해 명확히 정의되어 있지 않다.
    - 그래서 브라우저 마다 메모리 관리 방식이 조금 다를 수 있고, 아래의 두 가지 방법처럼 동작할 것이다.
        - 각자 다른 메모리 주소를 사용한다. 복사한 값을 새로운 메모리 주소에 저장한다.
        - 두 변수가 같은 메모리 주소를 사용한다. 같은 메모리 공간을 바라보고 있다가 식별자에 재할당되면 그 때, 새로운 공간의 메모리를 사용하게 된다.
            - 파이썬
- 같은 메모리 주소

## 11.2 객체

---

- 객체는 원시값과 다르게 동적으로 사용되기 때문에 사용 메모리를 예측하기 쉽지않다.
- 원시값과 다르게 메모리 관리가 된다.
- 자바스크립트 객체는 해시 테이블 유사하나 성능을 위해 해시 테이블보다 나은 방법을 사용한다.

<aside>
❓ 해시테이블?

</aside>

### 11.2.1 변경 가능한 값

- 객체의 식별자에 객체를 저장하고 있는 메모리 공간의 주소가 존재하고, 우리가 해당 객체에 간접적으로 접근하게 된다. → 식별자는 해당 객체를 참조하고 있다. = 가리키고 있다. = point 하고 있다.
- 객체를 수정하면 변수에 재할당을 한 것은 아니므로 변수의 참조값은 변하지 않는다.
- 동적인 객체를 가진 자바스크립트가 원시 값처럼 객체를 수정할 때마다 새로운 메모리 공간을 사용한다면 메모리 성능을 저하시키는 비효율적인 일이 일어날 수 있다. 이런 성능저하를 막기 위해 객체는 변경 가능한 값으로 설계되어 있다. (수정시 재할당이 일어나지 않는다.)
- 그렇기 때문에 여러개의 식별자가 하나의 객체를 공유할 수 있다는 단점이 있다.

```jsx
const one = {
  name: 'jiyeon',
  age: 28
};
const two = one;
console.log(two); // {name: 'jiyeon', age: 28}

two.name = 'seungah';
console.log(one); // {name: 'seungah', age: 28}

// 두 개의 식별자가 같은 객체를 바라보고 있어서 어느 하나에 수정을하면 동시에 적용된다.
// 이는 객체의 프로퍼티가 꼬일 수 있는 위험성이 존재한다.
```

- 얕은 복사와 깊은 복사의 차이는 기존 식별자와 새로운 식별자의 포인터가 같은 곳을 바라보냐, 다른 곳을 바라보냐의 차이가 아닌가?

<aside>
❓ 얕은복사? 깊은복사?
[https://velog.io/@th0566/Javascript-얕은-복사-깊은-복사](https://velog.io/@th0566/Javascript-%EC%96%95%EC%9D%80-%EB%B3%B5%EC%82%AC-%EA%B9%8A%EC%9D%80-%EB%B3%B5%EC%82%AC)

</aside>

### 11.2.2 참조에 의한 전달

- 기존 객체를 복사하면 참조값(메모리주소)가 복사본 식별자
- 자바스크립트엔 포인터가 존재하지 않는다.. ?!