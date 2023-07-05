# 인터페이스

### 인터페이스란?

인터페이스는 객체 타입을 정의할 때 사용하는 문법이다.

- 인터페이스로 타입을 정의할 수 있는 부분
  - 객체의 속성과 속성 타입
  - 함수의 파라미터와 반환 타입
  - 함수의 스펙 (파라미터 개수와 반환값 여부 등)
  - 배열과 객체를 접근하는 방식
  - 클래스

### 인터페이스를 이용한 객체 타입 정의
interface 라는 예약어를 이용하여 인터페이스를 선언한다.
```typescript
interface User {
    name: string;
    age: number;
}

var seho: User = { name: '세호', age: 36 };
```

### 인터페이스를 이용한 함수 타입 정의

```typescript
interface Person {
    name: string;
    age: number;
}

function getPerson(someone: Person): Person {
    return someone;
}
```

### 인터페이스의 옵션 속성(optional property)
인터페이스로 정의된 객체의 속성을 선택적으로 사용하고 싶을 때 옵션 속성을 사용한다.
속성에 `?`를 붙인다.

````typescript
interface Person {
    name?: string;
    age: number;
}
````

### 인터페이스 상속
인터페이스의 상속으로 타입 정의를 확장하는 방법
(상속: 객체 간 관계를 형성하는 방법, 상위(부모)클래스의 내용을 하위(자식)클래스가 물려받아 사용하거나 확장하는 기법)

- `extends` 예약어를 사용한다.
- 상속을 여러번 할 수 있음

```typescript
interface Hero {
    power: boolean;
}

interface Person extends Hero {
    name: string;
    age: number;
}

interface Developer extends Person {
    skill: string;
}

var ironman: Developer = {
    name: '아이언맨',
    age: 21,
    skill: '만들기',
    power: true
}
```
### 인터페이스를 이용한 인덱싱 타입 정의

인덱싱이란 객체의 특정 속성을 접근하거나 배열의 인덱스로 특정 요소에 접근하는 동작을 의미한다.
`user['name']`, `companies[0]`

#### 1. 배열의 인덱싱 타입 정의
````typescript
interface StringArray {
    [index: number]: string;
}

var companies: StringArray = ['삼성', '네이버', '구글']
````

#### 2. 객체의 인덱싱 타입 정의
```typescript
interface SalaryMap {
    [level: string]: string;
}

var salary: SalaryMap = {
    junior: '100원'
};

var money = salary['junior'];
```

### 인덱스 시그니처란?
정확히 속성 이름을 명시하지않고 속성 이름의 타입과 속성 값의 타입을 정의하는 문법이다.<br/>
객체의 속성 타입을 유연하게 정의할 때 사용한다.

- 어떤 속성이 제공될지 알 수 없어 코드 자동완성이 제공 되지 않는다. (힌트)

````typescript
interface SalaryInfo {
    [level: string]: string;
}

// 인덱스 시그니처를 정의하면 SalaryInfo 인터페이스로 정의한 객체에 유연하게 많은 속성을 추가할 수 있다.
var salary: SalaryInfo = {
    junior: '100원',
    mid: '400원',
    senior: '700원',
    ceo: '0원',
    newbie: '50원'
}

// 섞어서 정의 가능
// id와 name은 고정 프로퍼티
interface MixInfo {
    [level: string]: string;
    id: string;
    name: string;
}

````

#### 인덱스 시그니처는 언제 사용?
만약 객체의 속성 이름과 개수가 구체적으로 정해져있다면,
인터페이스에서 속성 이름과 속성 값의 타입을 명시해서 정의하고<br/>
속성 이름은 모르지만 속성 이름의 타입과 값의 타입을 아는 경우에는 인덱스 시그니처를 활용한다.

---

## Refer
쉽게 시작하는 타입스크립트 책 참고
