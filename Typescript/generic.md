# 제네릭 (generic)

제네릭은 타입을 미리 정의하지않고 사용하는 시점에 원하는 타입을 정의해서 쓸 수 있는 문법이다.
함수의 파라미터와 같은 역할을 한다.(인자에 넘긴값을 함수의 파라미터로 받아 함수내부에서 사용하는 방식)

> "타입을 넘기고 그 타입을 그대로 반환받는다"

## 제네릭의 기본 문법
```typescript
function getText<T>(text: T): T {
    return text;
}

getText<string>('hi');
```
--> getText함수를 호출할때 제네릭에 string타입을 할당한다.<br/>
--> (T라고 선언한 모든 부분에 string타입이 들어감)

## 제네릭을 사용하는 이유

### 1. 중복되는 타입 코드를 줄여준다.
타입을 미리 정의하지않고 코드를 호출할 때 타입을 정의해서 사용하므로 반복되는 타입 코드를 줄여준다.
### 2. any 타입으로 선언하면 발생하는 문제점을 제네릭으로 해결 가능
- any의 문제점 (타입스크립트의 장점들이 사라진다.)
  - 모든 타입을 다 취급할 수 있는 대신, 타입스크립트의 코드 자동 완성이나 에러의 사전방지 혜택을 받지못함
- 제네릭으로 선언하면?
  - 선언한 타입으로 추론되는 효과가 생김
### 3. 타입 때문에 불필요하게 중복되던 코드를 줄일 수 있고 타입이 정확하게 지정되면서 타입스크립트의 이점을 모두 가져갈 수 있다.

```typescript
function getBoolean(bool: boolean) {
    return bool;
}

function getArray(arr: []) {
    return arr;
}

// 등등 타입들이 다른 함수면 중복해서 함수를 선언해야함
```

## 인터페이스에 제네릭 사용하는법
제네릭은 함수뿐만 아니라 인터페이스에도 사용할 수 있다.

```typescript
interface Dropdown<T> {
    value: T;
}

var product: Dropdown<string>;
var stock: Dropdown<number>;
var address: Dropdown<{city: string; zipCode: string}>;
```
--> 인터페이스 이름 우측에 <T>를 붙여주고 인터페이스 내부 속성 중 타입을 제네릭으로 받을 곳에 T를 연결한다.

<b>타입을 유연하게 확장 가능, 비슷한 역할을 하는 타입 코드를 대폭 줄일 수 있음</b>

## 제네릭의 타입 제약

### 1. extends를 사용한 타입 제약
> 제네릭 선언부에 <T extends 타입>

````typescript
function test<T extends string>(param: T): T {
    return param;
}
````
--> string이 아닌 타입을 넣으면 에러 발생


- 쓰이는 용도 예시
```typescript
function lengthOnly<T extends { length: number }>(value: T) {
    return value.length;
}

lengthOnly('hi');
lengthOnly([1,2,3]);
lengthOnly({ title: 'abc', length: 123 });
```
제네릭의 타입 제약은 하나의 특정 타입 뿐만 아니라 특정 범위에 해당하는 여러개의 타입을 대상으로 지정할 수 있다.
