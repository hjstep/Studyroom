# join 함수와 find 함수

> 함수형 강의 섹션7 내용 정리 - 1<br/>
> (강의명: 함수형 프로그래밍과 JavaScript ES6+ 유인동 강사님)

## Array.prototype.join보다 다형성이 높은 join함수

Array.prototype.join은 배열만 가능한 함수인데
Array.prototype.join보다 더 다양성이 높은 join함수를 만들어보자.

```javascript
// 1. 
// join함수는 기본 구분자가 ','
const join = (sep=',', iter) => 
  reduce((a, b) => `${a}${sep}${b}`, iter); //sep(세퍼레이터)을 a와 b사이에 넣어줌

// 2.
// 파이프라인에서 조합성이 좋게 currry 적용
const join = curry((sep=',', iter) => 
  reduce((a, b) => `${a}${sep}${b}`, iter);

const queryStr = pipe(
	Object.entries,
	map(([k, v]) => `${k}=${v}`),
	join('&')
);

log(queryStr({limit: 10, offset: 10, type: 'notice'}));
// limit=10&offset=10&type=notice
```
함수형 프로그래밍을 하면 사이사이에 있는 함수들을 꺼내서 조합성, 재사용성이 높게 프로그래밍을 할 수 있다.

## find 함수

find는 첫번째로 만난 하나의 값을 꺼내주는 함수
take를 이용해서 결론을 만드는 함수다.(take를 통해 이터러블을 받는 함수)
````javascript
const users = [
    {age: 32},
    {age: 31},
    {age: 37},
    {age: 28},
    {age: 25},
    {age: 32},
    {age: 31},
    {age: 37}
  ];

// curry 커링적용
  const find = curry((f, iter) => go(
    iter,
    L.filter(f),
    take(1),
    ([a]) => a));

  log(find(u => u.age < 30)(users));

  go(users,
    L.map(u => u.age),// L.map은 지연적인 map
    find(n => n < 30),
    log);
````

find가 받는 값이 이터러블이기 때문에 
연산을 다 완료한 값을 줘도 사용이 가능하고
평가지연을 시킨 값을 줘도 사용가능하다.
```javascript
go(users,
    map(u => u.age),// 완료된 값을 줘도
    find(n => n < 30),
    log);

go(users,
    L.map(u => u.age),// L.map으로 지연적인 map을 줘도
    find(n => n < 30),
    log);

//find는 L.filter를 통해 지연해놓고 take로 연산하므로 어떤값줘도 가능
  const find = curry((f, iter) => go(
    iter,
    L.filter(f),
    take(1),
    ([a]) => a));

```
