- 클래스, 상속, 캡슐화를 위한 키워드가 없어서 객체 지향 언어가 아니다? (public, private, protected 등)
- 프로토타입 기반의 객체 지향 프로그래밍이다!

## 19.1 객체지향 프로그래밍

- 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임
- 어떤 사물이나 개념을 인식하는 철학적 사고를 프로그래밍에 접목하여 그 실체는 속성을 가지고 있다고 말한다.

> **추상화?**
다양한 실체의 속성 중 필요한 것만 표현하는 것
> 
- 객체
    - 속성을 통해 여러 개의 값을 하나의 단위로 구성한 복합적인 자료구조
    - 상태 데이터와 동작을 하나의 논리적인 단위로 묶은 복합적인 자료구조
        - 상태 데이터: 프로퍼티
        - 동작: 메서드
    - 다른 객체와 관계성을 맺을 수도 있다.
        - 메시지 주고받기
        - 데이터 처리
        - 상속

## 19.2 상속과 프로토타입

- 객체지향 프로그래밍의 핵심..!
- 자바스크립트는 프로토타입을 기반으로 상속을 구현해 불필요한 중복을 제거한다.
- 아래의 생성자 함수를 보자.

```jsx
// 지름이 다른 원 인스턴스를 생성하는 생성자 함수
function Circle(radius) {
	this.radius = radius;
	this.getArea = function() {
		return Math.PI * this.radius ** 2;
	};
}

const circle = new Circle(2);
const circle2 = new Circle(2);

console.log(circle.getArea() === circle2.getArea()); // true
console.log(circle.getArea === circle2.getArea); // false
// 같은 값을 도출하지만 각각의 함수는 다른 장소에 저장되어 있다.
```

- 위의 생성자 함수로 생성된 인스턴스들은 본인들의 `radius`  속성과 `getArea()` 메서드를 가진다.
    - 함수 객체들이 다른 메모리에 저장된다는 뜻이다.
- `상속` 을 사용하면 이러한 문제가 해결된다.

```jsx
function Circle(raiuds) {
	this.radius = radius;
}

// Circle의 프로토타입에 상속해줄 값을 추가한다
Circle.prototype.getArea = function() {
	return Math.PI * this.radius ** 2;
};

const circle = new Circle(1);
const circle2 = new Circle(2);

console.log(circle.getArea === circle2.getArea); // true
```
