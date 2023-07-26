# 값으로 다루는 Promise 활용

> 함수형 강의 섹션8 내용 정리 <br/>
> (강의명: 함수형 프로그래밍과 JavaScript ES6+ 유인동 강사님)

Promise가 일급의 성질을 가지고있다는 점(값으로 다룰 수 있음)을 통해 다양하게 활용할 수 있다.

- 일급 활용

```javascript
const go1 = (a, f) => f(a);
const add5 = a => a+ 5;
go1(10, add5);// 15
```

<b>go1함수가 정상으로 동작하려면 ?</b>

f라는 함수가 동기적으로 작동해야하며 a라는 값도 동기적으로 즉시평가되는 값이여야한다<br/>
비동기 상황이 일어난 (일급값이 아닌) 일반값 (Promise가 아닌값)이 go1 인자에 들어와야 값을 잘 적용할 수 있게된다.<br/>
만약 go1 함수 인자값이 프로미스값이라면 정상 동작할 수 없게됨

```javascript
go1(Promise.resolve(10), add5));// [object Promise]5
```

- go1함수가 프로미스인자값도 정상으로 동작하게 만드려면?

```javascript
go1(10, add5);
go1(Promise.resolve(10), add5));// 정상으로 동작하게 만들고싶다.
```

```javascript
//delay100으로 예시
const delay100 = a => new Promise(resolve => setTimeout(() => resolve(a), 100));

// 프로미스인자값도 잘 동작할 수 있도록 변경
const go1 = (a, f) => a instanceof Promise ? a.then(f) : f(a);
const add5 = a => a + 5;

go1(delay100(10), add5));

var r = go1(10, add5);
console.log(r);

var r2 = go1(delay100(10), add5);
r2.then(log);
```

go1 함수에 전달된 첫번째 인자 a가 Promise라면 then을 한 후에 두번째 인자 함수 f를 적용<br/>
—> 다형성을 지원하는 go1함수

- 이제 구조를 똑같이 할 수 있다.

```javascript
const n1 = 10;                // 로그까지 즉시 실행된 최종결과를 받게됨
log(go1(go1(n1, add5), log)); // undefined
														  // 15

const n2 = delay100(10);      // 프로미스를 계속해서 이어주게됩니다.
log(go1(go1(n2, add5), log)); // Promise 객체
															// 15
```

어떤일을 한 결과의 상황을 일급값으로 만들어 지속적으로 연결해나갈수 있도록 하는것이 프로미스의 가장 중요한 특징 중 하나이다.