## 20.1 strict mode란?

- 키워드로 변수 선언을 하지 않으면 암묵적 전역이 일어나고 해당 변수를 참조하면 에러가 발생하지 않는다.
- ES5부터 지원
    - 언어의 문법을 더욱더 엄격하게 적용시킨다.
- strict mode보다 ESLint를 선호
    - strict mode + 코딩 컨벤션

## 20.2 strict mode의 적용

- 전역의 선두, 함수 몸체의 선두 `‘use strict’;` 추가

## 20.3 전역에 strict mode를 적용하는 것은 피하자

- 스크립트 단위로 실행된다.
    - 다른 스크립트에는 영향을 주지 않는다.
- 즉시 실행 함수로 스코프를 구분하고 strict mode 적용

## 20.4 함수 단위로 strict mode를 적용하는 것도 피하자

- 번거롭다.
- 중첩 함수의 경우 또 번거로워진다.

## 20.5 strict mode가 발생시키는 에러

- 암묵적 전역
    - 선언하지 않은 변수 참조시 `ReferenceError` 발생
- `delete` 를 이용해 변수, 함수, 매개변수 삭제시 `SyntaxError` 발생
- 중복된 매개변수 이름 사용시 `SyntaxError` 발생
- 성능과 가독성이 나빠지는 `with` 문의 사용시 `SyntaxError` 발생

## 20.6 strict mode 적용에 의한 변화

- 일반 함수 내부에서 this 사용시 this에 undefined 바인딩
    - 그러나 따로 에러는 발생하지 않음
- 매개변수에 전달된 인수를 재할당하여 변경하여도 무시된다.