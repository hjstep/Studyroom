# BigInt 타입

## BigInt 타입이란?

원시형 타입 중 하나로, 불변성을 유지하고
Number 타입 최대값보다 2^53 - 1 큰 정수를 표현할 수 있다.
(금융데이터로 주로 사용함)

## BigInt 특징

- new 키워드를 통해 생성되지 않는다.
- typeof 연산자 결과값은 bigint
    - `typeof BigInt(’1’) === ‘bigint’`
- Number와 일치하지는 않지만, 동등하다.
```javascript
BigInt == Number // true

BigInt === Number //false
```

## 생성법

- 리터럴
    - 정수 리터럴의 뒤에 n을 붙인다
    - 예시: `10n`
- 함수호출
    - BigInt() 함수 호출
    - 예시: `BigInt(10)`

## 주의사항

- Math 객체의 메서드와 함께 사용할 수 없다.
    - Math객체는 Number타입만 지원한다.
- Number 값과 연산에서 혼용하여 사용할 수 없다.
    - 만약, 연산을 하고 싶다면 형변환 해야한다. <br/>그러나 BigInt를 Number로 형변환하면 정확성을 잃을 수 있다.
