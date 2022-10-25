# 빌트인 객체

## 21.1 자바스크립트 객체의 분류

- 표준 빌트인 객체
    - ECMAScript 사양에 정의된 객체
    - 전역 객체의 프로퍼티
- 호스트 객체
    - 실행 환경에 따라 추가로 제공되는 객체
        - 브라우저 또는 Node.js
- 사용자 정의 객체

## 21.2 표준 빌트인 객체

- 모두 인스턴스를 생성할 수 있는 생성자 함수 객체
    - 프로토타입 메서드와 정적 메서드 제공
    - Math, Reflect, JSON 제외
        - 얘네는 정적 메서드만 제공

## 21.3 원시값과 래퍼 객체

- 원시값은 객체가 아니지만 객체처럼 동작한다.
    - 엔진에서 암묵적으로 연관된 객체를 생성하여 해당 프로퍼티에 접근/호출 후 원시값으로 되돌린다.
        - 사용된 임시 객체는 가비지 콜렉터가 주서간다.
    - 엔진에 의해 임시로 객체가 되는 친구들을 `래퍼 객체`라고 부른다.
- Symbol()은 생성자 함수가 아니므로 래퍼 객체를 만들 수 없다.
    - null과 undefined도 동일하다.

```jsx
const str = 'hello';

console.log(str.length); // 5
```

## 21.4 전역 객체

- 최상위 객체
    - 브라우저 - window, self, this, frames
    - Node.js - global
    - ES11에서 `globalThis`로 통일되었다.
- 개발자가 의도적으로 생성할 수 없다.
- 전역 객체 참조는 전역 객체를 가리키는 식별자를 사용하지 않아도 된다.
- let, const 보이지 않는 블록 내에 존재, var 키워드는 전역 객체의 프로퍼티가 된다.

### 21.4.1 빌트인 전역 프로퍼티

- Infinity - 무한대
- NaN - Not-a-Number
- undefined - 원시 타입

### 21.4.2 빌트인 전역 함수

- eval
    - 문자열을 인수로 받아 표현식인 문은 값을 생성한다.
    - 표현식이 아닌 문은 런타임에 실행한다.
    - 보안 취약, 최적화가 되지 않으며 처리 속도가 느려 사용 금지
- isFinite
    - 유한수, null : true
    - 무한수, NaN : false
- isNaN
    - 숫자가 아닌 타입 : true
    - 숫자 타입 : false
- parseFloat
    - 부동 소수점 숫자 → 실수 반환
- parseInt
    - 문자열, 소수점 → 정수로 반환
    - 두번째 인수는 기수 전달
        - `parseInt(’10’, 2);` : ‘10’을 2진수로 해석하고 10진수로 정수 반환
    - 기수로 변환하는 다른 방법
        - Number.prototype.toStiring(기수) : 인수로 받는 값의 기수로 문자열로 반환
    - 해석할 수 없는 문자열 이후는 모두 무시되어 평가된다.
- encodeURI / decodeURI
    - 인코딩
        - 문자들의 이스케이프 처리하는 것
            - 알파벳과 숫자는 제외
        - 시스템에서도 읽을 수 있는 아스키 문자 셋으로 변경한다.
    - 디코딩
        - 인코딩되기 전 단계로 돌리는 것

```jsx
const uri ="https://velog.io/@lhr4884/모도코에서-코드-리뷰-하실분";

const enc = encodeURI(uri);

console.log(enc); // https://velog.io/@lhr4884/%EB%AA%A8%EB%8F%84%EC%BD%94%EC%97%90%EC%84%9C-%EC%BD%94%EB%93%9C-%EB%A6%AC%EB%B7%B0-%ED%95%98%EC%8B%A4%EB%B6%84
```

- encodeURIComponent / decodeURIComponent
    - URI 구성 요소를 인수로 받아 인코딩

### 21.4.3 암묵적 전역

- 이전 챕터에서 이미 나온 내용!