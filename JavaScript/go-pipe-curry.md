# 코드를 값으로 다루어 표현력 높이기

> 함수형 강의 섹션4(코드를 값으로 다루어 표현력 높이기) 내용 정리 <br/>
> (강의명: 함수형 프로그래밍과 JavaScript ES6+ 유인동 강사님)

### 코드를 값으로 다루기
함수형 프로그래밍에서는 코드를 값으로 다루는 아이디어를 많이 사용한다.
코드를 값으로 다룰 수 있기 때문에 어떤 함수가 코드인 함수를 받아서 평가하는 시점을 
원하는 대로 다룰 수가 있기 때문에 어떤 코드의 표현력을 높인다든지 굉장히 많은 아이디어들을 가지고 있다.


### 커링 

커링은 함수형 프로그래밍의 원칙과 철학에 기반한 기술이다.
함수형 프로그래밍은 상태 변경과 가변 데이터보다는 순수 함수와 불변 데이터를 중심으로 프로그래밍하는 패러다임이다. 
이 패러다임은 코드의 가독성, 유지 보수성, 테스트 용이성 등을 향상시킬 수 있다.

## go 함수
go 라는 함수를 만들어 중첩되어있는 함수를 읽기 쉽게 바꿔보는 실습

#### [ as-is ]

```javascript
const products = [
    {name: '반팔티', price: 15000},
    {name: '긴팔티', price: 20000},
    {name: '핸드폰케이스', price: 15000},
    {name: '후드티', price: 30000},
    {name: '바지', price: 25000}
  ];

  const add = (a, b) => a + b;

log(
  reduce(
    add,
    map(
      p => p.price,
      filter(p => p.price < 20000, products),
    ),
  ),
);
```

#### [ to-be ]
```javascript
const go = (...args) => reduce((a, f) => f(a), args);

// 예시
  go(
    add(0, 1),
    a => a + 10,
    a => a + 100,
    log);
  // 111

// go 함수를 이용하여 읽기 좋은 코드로 만들기
log(
    reduce(
        add,
        map(p => p.price,
            filter(p => p.price < 20000, products))));

// 위의 코드보다 코드양이 많아지고 간결하진않지만, 읽기 좀 편해짐
go(
    products,
    products => filter(p => p.price < 20000, products),
    products => map(p => p.price, products),
    prices => reduce(add, prices),
    log);

```

> reduce 리듀스 함수 <br/>
특정 리스트를 출력해 나가는 코드를 작성할 때 reduce라는 함수를 사용하면 재미있고 쉽게 만들어 나갈 수가 있음

## pipe 함수
go 함수를 내부적으로 구현한 함수를 만드는 실습 

- pipe 함수는 go 함수와 다르게 함수를 리턴하는 함수임
- go 함수는 함수들과 인자를 전달해서 어떤 값을 평가하는데 사용한다면, pipe함수는 함수들이 나열되어 있는 합성된 함수를 만드는 함수
- pipe함수는 내부적으로 go를 사용하도록 구현한 함수
- pipe 함수는 첫번째 인자에 (a, b) ⇒ a+b를 줘서 인자 두 개를 평가해서 1로 만들어서 시작되도록 만들 수 있음

## curry 함수

- 함수를 값으로 다루면서 받아둔 함수를 내가 원하는 시점에 평가시키는 함수이다.
- 함수를 받아서 함수를 이탈하고 인자를 받아서 인자가 원하는 개수만큼의 인자가 들어왔을 때 받아두었던 함수를 나중에 평가시키는 함수이다.

```javascript
/*
만약에 이 함수에 인자가 두 개 이상 전달되었을 때라는 것을 뜻하는 것은 이제 여기에 _.length가 있을 때일 겁니다. 
그래서 만약에 여기에 _.length가 있다면 있다면 받아둔 함수를 즉시 실행을 하고 즉시 실행을 하고 만약에 아니라면 다시 한번 함수를 리턴합니다. 
그래서 그 이후에 그 이후에 들어올 값들을 받아보고요.

이렇게 받아서 이제 그때는 함수를 실행하는 그런 함수입니다. 
또 미리 받아놨던 a와 새로 받은 이제 이 인자를 또 전달을 하는 거죠. 
그래서 다시 한번 간단히 설명드리면 함수를 받아서 일단 함수를 리턴합니다. 
그리고 그렇게 리턴된 함수가 실행되었을 때 인자가 만약에 두 개 이상이라면 두 개 이상이라면 받아둔 함수를 즉시 실행을 하고 만약에 인자가 두 개 두 개보다 작다면 함수를 다시 리턴한 후에 그 이후에 받은 인자들을 합쳐서 이제 실행하는 그런 함수입니다.
*/

const curry = f =>
  (a, ..._) => _.length ? f(a, ..._) : (..._) => f(a, ..._);
```

## go + curry 실습

```javascript
// curry 
const mult = curry((a, b) => a * b);
log(mult(3)(2));

const mult3 = mult(3);
log(mult3(10));
log(mult3(5));
log(mult3(3));

// filter, map, reduce에 curry 적용하여 go함수를 간결하게 만들기
const products = [
    {name: '반팔티', price: 15000},
    {name: '긴팔티', price: 20000},
    {name: '핸드폰케이스', price: 15000},
    {name: '후드티', price: 30000},
    {name: '바지', price: 25000}
  ];
const add = (a, b) => a + b;

go(
  products,
  filter(p => p.price < 20000),
  map(p => p.price),
  reduce(add),
  log);

/**** filter, map, reduce에 curry 적용한 소스 *************************/
const log = console.log;
const go = (...args) => reduce((a, f) => f(a), args);
  go(
    products,
    filter(p => p.price < 20000),
    map(p => p.price),
    reduce(add),
    log);


const curry = f =>
  (a, ..._) => _.length ? f(a, ..._) : (..._) => f(a, ..._);

const map = curry((f, iter) => {
  let res = [];
  for (const a of iter) {
    res.push(f(a));
  }
  return res;
});

/* go(products, map(p=>p.price)); 해석
map(p=>p.price)는 인자가 1개니까 _.length 조건식 false라서 (..._) => f(a, ..._)임
이게 f = (f, iter) => {
  let res = [];
  for (const a of iter) {
    res.push(f(a));
  }
  return res;
}
a = p=> p.price
..._ => products

((a, ..._) => {
  let res = [];
  for (const it of iter) {
    res.push(a(it));
  }
  return res;
})(p=>p.price, products)
*/

const filter = curry((f, iter) => {
  let res = [];
  for (const a of iter) {
    if (f(a)) res.push(a);
  }
  return res;
});

const reduce = curry((f, acc, iter) => {
  if (!iter) {
    iter = acc[Symbol.iterator]();
    acc = iter.next().value;
  }
  for (const a of iter) {
    acc = f(acc, a);
  }
  return acc;
});

```
