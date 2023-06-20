# 연산자 (operator)

### 용어
- 피연산자 operand : “값”이라는 명사의 역할, 연산의 대상
- 연산자 operator : “피연산자를 연산하여 새로운 값을 만든다”라는 동사의 역할,
  하나 이상의 표현식을 대상으로 산술, 할당, 비교, 논리, 타입, 지수 연산 등을 수행해 하나의 값을 만든다.

### 특징
- 피연산자는 표현식이어야 한다.
- 연산자 + 피연산자 조합으로 이루어진 연산자 표현식은 값으로 평가될 수 있는 표현식이어야 한다.
- 피연산자는 연산의 대상이 되어야하므로 값으로 평가할 수 있어야 하며, 연산자는 값으로 평가된 피연산자를 연산해 새로운 값을 만든다.

---

## 연산자의 종류
크게 9가지 종류 (산술, 할당, 비교, 삼항조건, 논리, 쉼표, 그룹, 타입, 지수 연산자)

### 1. 산술 연산자  arithmetic operator
- 피연산자를 대상으로 수학적 계산을 수행해 새로운 숫자 값을 만든다.
- 산술 연산이 불가능한 경우 NaN을 반환함.
- 피연산자의 개수에 따라 이항 산술 연산자와 단항 산술 연산자로 구분할 수 있다.

**1) 이항binary 산술 연산자**

- 2개의 피연산자를 산술 연산하여 숫자값을 만든다. 부수효과없음 (어떤 산술연산을 해도 피연산자의 값이 바뀌지않고 새로운값을만듦)
- `+ - * / %` 덧셈 뺄셈 곱셈 나눗셈 나머지

**2) 단항unary 산술 연산자**

- 1개의 피연산자를 산술 연산하여 숫자값을 만든다. `++`와 `--`는 부수효과 O
- `++` 증가 (부수효과 O, 피연산자 암묵적 할당)
- `--`  감소 (부수효과 O, 피연산자 암묵적 할당)
- `+`  아무효과없음, 음수를 양수로 반전하지도않음 (부수효과 X, 암묵적 타입변환 발생)
- `-`  양수를 음수로, 음수를 양수로 반전한 값 반환함 (부수효과 X,  암묵적 타입변환 발생)

**2-1) 증가/감소(++/--) 연산자**

(1)  피연산자의 값을 변경하는 부수 효과가 있다. 피연산자의 값을 변경하는 암묵적 할당이 이뤄진다.

(2) 위치

- 전위 증가/감소 연산자: 피연산자의 앞에 위치, 먼저 피연산자의 값을 증가/감소시킨 후 다른 연산 수행

- 후위 증가/감소 연산자: 피연산자의 뒤에 위치, 먼저 다른 연산 수행 후 피연산자의 값을 증가/감소시킨다

**2-2) `+` 단항 연산자**

(1) 피연산자가 숫자타입이 아니면 피연산자를 숫자 타입으로 변환하여 반환한다.<br/>
→ 부수효과 없음, 피연산자를 변경하는것이 아니고 숫자타입으로 변환한 값을 새롭게 생성해서 반환한다.

```javascript
// 아무 효과 없음
+10; // 10 

+true; // 1
+false; // 0
+'Hello';// NaN
```

**2-3) `-` 단항 연산자**

(1) 피연산자의 부호를 반전한다.

(2) 피연산자가 숫자타입이 아니면 피연산자를 숫자 타입으로 변환하여 반환한다.<br/>
→ 부수효과 없음, 피연산자를 변경하는것이 아니고 숫자타입으로 변환한 값을 새롭게 생성해서 반환한다.

```javascript
-(-10); // 10

-true; // -1
-'Hello'; // NaN
```

**3) 문자열 연결 연산자 `+`**

- + 연산자는 피연산자 중 하나 이상이 문자열인 경우 문자열 연결 연산자로 동작한다.
- 암묵적 타입 변환implicit coercion (= 타입 강제 변환 type coercion)이 발생한다.
  : 개발자의 의도와는 상관없이 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환되기도 한다.

```tsx
// 문자열 연결 연산자 
'1' + 2 // '12'

// 산술 연산자
1 + 2; // 3

// 암묵적 타입 변환되어 연산이 수행됨.
1 + true; // 2 (true는 1)
1 + false; // 1 (false는 0)
1 + null; // 1 (null은 0)
1 + undefined; // NaN (undefined는 숫자로 타입 변환되지 않음)
+undefined; // NaN
```

### 2. 할당 연산자 assignment operator : 우항 피연산자 평가 결과를 좌항 변수에 할당한다. 부수효과 O

좌항의 변수에 값을 할당하므로 변수 값이 변하는 부수효과가 있다.

| 할당 연산자 | 예시 | 동일 표현  | 부수효과 |
| --- | --- | --- | --- |
| = | x = 5 | x = 5 | O |
| += | x += 5 | x = x + 5 | O |
| -= | x -= 5 | x = x - 5 | O |
| *= | x *= 5 | x = x * 5 | O |
| /= | x /= 5 | x = x / 5 | O |
| %= | x %= 5 | x = x % 5 | O |

- 할당문은 표현식인 문이다.
  할당문 x=10은 x에 할당된 숫자값 10으로 평가된다. 따라서 할당문을 다른 변수에 할당할 수 있다. (→ 연쇄 할당 가능)

```tsx
// 연쇄 할당
var a,b,c;
a = b = c = 0; // 여러 변수에 동일한 값을 연쇄 할당함

console.log(a,b,c); // 0 0 0 
```

### 3. 비교 연산자 comparison operator : 좌항과 우항의 피연산자를 비교한 다음, 그 결과를 불리언 값으로 반환한다.

비교 연산자는 if문, for문과 같은 제어문의 조건식에서 주로 사용된다.

**1) 동등/일치 비교 연산자**

- 동등 비교 loose equality 연산자와 일치 비교 strict equality 연산자의 공통점과 차이점
  - 공통점) 좌항과 우항의 피연산자가 같은 값으로 평가되는지 비교해 불리언 값을 반환한다.
  - 차이점) 비교하는 엄격성 정도가 다르다.
    - 동등 비교 연산자는 “느슨한 비교”
    - 일치 비교 연산자는 “엄격한 비교”

| 비교 연산자 | 의미 | 예시 | 설명 | 부수효과 |
| --- | --- | --- | --- | --- |
| == | 동등 비교 | x == y | x와 y의 값이 같음 | X |
| === | 일치 비교 | x === y | x와 y의 값과 타입이 같음 | X |
| != | 부동등 비교 | x != y | x와 y의 값이 다름 | X |
| !== | 불일치 비교 | x !== y | x와 y의 값과 타입이 다름 | X |

- 동등 비교(==) 연산자
  - 좌항과 우항의 피연산자를 비교할 때 먼저 암묵적 타입 변환을 통해 타입을 일치시킨 후 같은 값인지 비교한다.
  - 좌항과 우항의 피연산자가 타입이 다르더라도 암묵적 타입 변환 후 같은 값일 수 있다면 true를 반환한다.
  - 동등 비교 연산자는 예측하기 어려운 결과를 만들어내므로 일치 비교 연산자를 사용하자.
- 일치 비교(===) 연산자
  - 좌항과 우항의 피연산자가 타입, 값 모두 같은 경우만 true를 반환한다.
  - 암묵적 타입 변환을 하지 않고 값을 비교한다.
  - 일치 비교 연산자는 예측하기 쉽다.
  - 주의1) NaN
    - NaN은 자신과 일치하지 않는 유일한 값이다. `NaN !== NaN`
    - 숫자가 NaN인지 조사하려면 빌트인 함수 Number.isNaN을 사용한다.
  - 주의2) 양의 0과 음의 0
    - 양의 0과 음의 0을 비교하면 true다.
    - `0 === -0 // true`
    - `0 == -0 // true`
  - 해결) [Object.is](http://Object.is) 메소드 이용할것.
    - [Object.is](http://Object.is) 메소드는 예측 가능한 정확한 비교결과를 반환한다. 양의 0과 음의 0도 비교할 수 있고, NaN 비교하면 true 반환한다.
    - `Object.is(-0, +0); // false`
    - `Object.is(NaN, NaN); // true`

- 부동등 비교연산자(!=)는 동등 비교 연산자(==)와 반대
- 불일치 비교연산자(!==)는 일치 비교 연산자(===)와 반대

**2) 대소 관계 비교 연산자**

피연산자의 크기를 비교하여 불리언 값을 반환한다.

| 대소 관계 비교 연산자 | 예시 | 설명 | 부수효과 |
| --- | --- | --- | --- |
| > | x > y  | x가 y보다 크다 | X |
| < | x < y | x가 y보다 작다 | X |
| >= | x >= y | x가 y보다 크거나 같다 | X |
| <= | x <= y | x가 y보다 작거나 같다 | X |

### 4. 삼항 조건 연산자 ternary operator : 조건식의 평가 결과에 따라 반환할 값을 결정한다. 부수효과는 X

조건식 ? 조건식이 true일때 반환할 값 : 조건식이 false일때 반환할 값

- 첫번째 피연산자(조건식)이 true로 평가되면 두번째 피연산자를 반환하고, false로 평가되면 세번째 피연산자를 반환한다.
- 만약 조건식 평가결과가 불리언값이 아니면 불리언 값으로 암묵적 타입 변환된다.
- 삼항 조건 연산자 표현식은 표현식인 문이므로 값처럼 사용할 수 있지만 if…else문은 표현식이 아닌 문이므로 값처럼 사용할 수 없다.
- 용도: 조건에 따라 어떤 값을 결정해야 할때 사용. 만약 조건에 따라 수행해야할 문이 여러개라면 if…else문이 더 가독성 좋다.

### 5. 논리 연산자 logical operator : 피연산자를 논리 연산한다.

| 논리 연산자 | 의미       | 부수효과 |
|--------|----------| --- |
| 논리합기호  | 논리합(OR)  | X |
| &&     | 논리곱(AND) | X |
| !      | 부정(NOT)  | X |

```tsx
true || false; // true

false && true; // false

!true; // false
!false; // true
```

- 논리 부정(!) 연산자는 불리언값을 반환한다. 만약 피연산자가 불리언 값이 아니면 불리언 타입으로 암묵적 타입 변환된다.
- 논리합(||) 또는 논리곱(&&) 연산자는 2개의 피연산자 중 어느 한쪽으로 평가된다. 평가 결과가 불리언 타입이 아닐수도있음
- 드 모르간의 법칙

### 6. 쉼표 연산자 (, ) : 왼쪽 부터 차례대로 피연산자를 평가하고 마지막 피연산자 평가가 끝나면 마지막 피연산자 평가결과를 반환한다.

```tsx
var x, y, z;

x = 1, y = 2, z = 3;// 3
```

### 7. 그룹 연산자 ( ‘( 소괄호 )’ ) : 소괄호로 피연산자를 감싸며, 가장 먼저 평가한다. 그룹연산자는 연산자 우선순위가 가장 높다.

그룹 연산자를 사용하면 연산자 우선순위를 조절할 수 있다.

### 8. typeof 연산자 : 피연산자의 데이터 타입을 문자열로 반환한다.

- typeof 연산자는 8가지 “string, number, boolean, undefined, symbol, object, function, bigint” 중 하나를 반환한다.
- null을 반환하는 경우는 없다. 함수는 function을 반환함.
- typeof 연산자가 반환하는 문자열은 7개의 데이터 타입과 정확히 일치하지는 않는다.
- 선언하지않은 식별자를 typeof 연산하면 **undefined**를 반환한다. (ReferenceError가 아닌..)

```tsx
typeof null; // object "null이 아닌 object를 반환한다. <-- ***주의 자바스크립트의 첫번째 버그!!!!!!!!!!!!!!!!!!!!****

//null 타입인지 확인할때는 일치 연산자 사용하자
foo === null;

typeof undeclared; // undefined
```

### 9. 지수 연산자 : 좌항 피연산자를 밑base으로,  우항의 피연산자를 지수exponent로 거듭 제곱하여 숫자값을 반환한다.



- 음수를 거듭제곱의 밑으로 사용하려면 괄호로 묶어야한다.
- 지수연산자의 결합 순서는 우항에서 좌항이다. (우결합성)
  - 2 ** 3 ** 2; // 512
- 지수연산자와 할당연산자를 결합
  - num **= 2;
- 지수연산자는 이항연산자 중에서 우선순위가 가장 높다.

```tsx
2 ** 0; // 1
2 ** 2; // 4

/* 음수를 거듭제곱의 밑으로 사용하려면 괄호로 묶어야한다. */
(-5) ** 2; // 25

/* Math.pow 메서드로도 가능 */

Math.pow(2, 2); // 4
Math.pow(2, 0); // 1
```

---

## 연산자의 부수효과

- 부수효과가 있는 연산자는 다른 코드에 영향을 준다.
- 부수효과는 피연산자의 값이 바뀐다는 뜻
- `할당 연산자(=), 증가/감소 연산자(++/--), delete 연산자`

## 연산자의 우선순위

여러개의 연산자로 이뤄진 문이 실행될 때 연산자가 실행되는 순서를 말한다.

우선순위가 높을 수록 먼저 실행됨

| 우선순위 | 연산자                                                                    |
|------|------------------------------------------------------------------------|
| 1    | ( ) 소괄호                                                                |
| 2    | new(매개변수 존재), .// 멤버접근, [ ] //계산된프로퍼티접근, ( ) //함수호출, ?. // 옵셔널 체이닝 연산자 |
| 3    | new(매개변수 미존재)                                                          |
| 4    | x++, x-- 후위증감연산자                                                       |
| 5    | !x, +x, -x, ++x, --x, typeof, delete                                   |
| 6    | ** // 이항 연산자중 가장 우선순위 높다                                               |
| 7    | *  /  %                                                                |
| 8    | +  -                                                                   |
| 9    | < , <= , > , >= , in, instanceof                                       |
| 10   | ==  !=  ===   !==                                                      |
| 11   | ?? //null 병합 연산자                                                       |
| 12   | &&                                                                     |
| 13   | 논리합                                                                    |                                                                   ||`                                                                     |                                                                       |
| 14   | ?   …  :   … 삼항조건연산자                                                   |
| 15   | = += -= 등 할당 연산자                                                       |
| 16   | , 쉼표 연산자                                                               |



## 연산자의 결합순서

연산자 결합순서란 연산자의 어느 쪽(좌항 or 우항)부터 평가를 수행할 것인지를 나타내는 순서이다.

| 결합 순서 | 연산자 |
| --- | --- |
| 좌항 → 우항 | + - / % < <= > >= && || . [] () ?? ?. in instanceof |
| 우항 → 좌항 | ++ -- 할당연산자 !x +x -x ++x --x typeof delete 삼항조건연산자, **지수연산자 |