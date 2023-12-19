# 추상연산 OrdinaryObjectCreate

> 추상연산 abstract operation
> ECMAScript 사양에서 내부 동작의 구현 알고리즘을 표현한 것

## OrdinaryObjectCreate
Object.prototype을 프로토타입으로 갖는 빈객체를 생성한다


### 1. Object 생성자 함수 사양
Object 생성자 함수에 인수를 전달하지 않거나 undefined, null을 인수로 전달하면서 호출하면 내부적으로 추상 연산 OrdinaryObjectCreate를 호출하여 Object.prototype을 프로토타입으로 갖는 빈객체를 생성한다

### 2. 객체 리터럴 평가
추상연산 OrdinaryObjectCreate를 호출하여 빈객체를 생성하고 프로퍼티를 추가한다

### Object 생성자 함수 호출 vs 객체 리터럴의 평가
- 추상연산 OrdinaryObjectCreate를 호출하여 빈객체를 생성한다.
- 세부내용은 다르다(new.target 확인이나 프로퍼티를 추가하는 처리 등 )
- 객체 리터럴에 의해 생성된 객체는 Object 생성자 함수가 생성한 객체가 아니지만, 가상적인 생성자 함수 Object를 생성자 함수로 생성하므로(Object.prototype 접근가능) 본질적으로 큰 차이는 없음