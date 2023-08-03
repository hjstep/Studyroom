# go, pipe, reduce에서 비동기 제어

> 함수형 강의 섹션8 내용 정리 <br/>
> (강의명: 함수형 프로그래밍과 JavaScript ES6+ 유인동 강사님)

```jsx
// 다음과 같이 실행하면
// [object Promise]100010000 
// go 함수를 고쳐야한다.
go(1,
	a => a + 10,
	a => Promise.resolve(a + 100),
	a => a + 1000,
	a => a + 10000,
  log);
```

- go 함수 고치기

```jsx
// go함수는 즉시실행만 하고있어서 reduce만 고쳐주면 됨
const go = (...args) => reduce((a, f) => f(a), args);
```

- pipe 함수 고치기

```jsx
// pipe함수도 go함수를 사용하고 있어서 reduce만 고쳐주면 됩니다.
const pipe = (f, ...fs) => (...as) => go(f(...as), ...fs);
```

- reduce 고치기
    - before

```jsx
const reduce = curry((f, acc, iter) => {
	if (!iter) {
		iter = acc[Symbol.iterator]();
		acc = iter.next().value;
	} else {
		iter = iter[Symbol.iterator]();
	}
	let cur;
	while (!(cur = iter.next()).done) {
		const a = cur.value;
		acc = f(acc, a);
	}
	return acc;
});
```

acc = f(acc, a); // f 함수에 프로미스가 전달된 상태로 진행이 되기때문에

>> f함수가 프로미스의 값을 기다려서 만들어진 값으로 변환을 해주어야 연속적으로 실행을 읽을 수 있다.

- 해결 1 - 아래코드는 안좋은 해결법이다.

중간에 프로미스를 만나면 계속 프로미스 체인이 일어나기때문에 연속적으로 비동기가 일어나게됩니다

프로미스 이후 라인은 동기적으로 하나의 콜스택에서 실행되기 바랬다면, 아래처럼 `acc = acc instanceof Promise ? acc.then(acc => f(acc, a)) : f(acc, f);` 이렇게 하면 그런일을 처리하지 못하게된다. 그리고 이런 함수합성이 코드에 많다면 불필요한 로드가 생기기때문에 성능저하 발생한다.

```jsx
const reduce = curry((f, acc, iter) => {
	if (!iter) {
		iter = acc[Symbol.iterator]();
		acc = iter.next().value;
	} else {
		iter = iter[Symbol.iterator]();
	}
	let cur;
	while (!(cur = iter.next()).done) {
		const a = cur.value;
		acc = acc instanceof Promise ? acc.then(acc => f(acc, a)) : f(acc, f);
	}
	return acc;
});
```

- 해결2

프로미스가 아닐때는 동기적으로 while문 다음을 실행할 수 있도록 구성하는게 좋음

```jsx
const reduce = curry((f, acc, iter) => {
	if (!iter) {
		iter = acc[Symbol.iterator]();
		acc = iter.next().value;
	} else {
		iter = iter[Symbol.iterator]();
	}
	return function recur(acc) {
			let cur;
			while (!(cur = iter.next()).done) {
				const a = cur.value;
				acc = f(acc, a);// f함수를 적용해본 후에 결과값이 만약 프로미스라면 아래 if문 진행
				if (acc instanceof Promise) return acc.then(recur);
			}
			return acc;
	} (acc);
});

```

```jsx
go(1, //a
	a => a + 10, //b
	a => Promise.resolve(a + 100), // 이걸 제외한 콜스택
	a => a + 1000,// c
	a => a + 10000,//d
  log);// e
```

`a => Promise.resolve(a + 100),`를 제외한 나머지 a,b,c,d,e 라인은 하나의 콜스택으로 동작하기때문에 성능적으로 해결1보다 좋다.

- 해결3

`if (!iter) {
 iter = acc[Symbol.iterator]();
 acc = iter.next().value;
}`이 코드때문에 해결2로 하면 처음 초기값이 프로미스일때 문제가 된다

```jsx
// acc가 프로미스면 풀어서 전달하고 아니라면 그냥 f(a);전달(하나의콜스택,동기적으로 동작하도록)
const go1 = (a, f) => a instanceof Promise ? a.then(f) : f(a);

const reduce = curry((f, acc, iter) => {
	if (!iter) {
		iter = acc[Symbol.iterator]();
		acc = iter.next().value;
	} else {
		iter = iter[Symbol.iterator]();
	}
	return go1(acc, function recur(acc) {
			let cur;
			while (!(cur = iter.next()).done) {
				const a = cur.value;
				acc = f(acc, a);// f함수를 적용해본 후에 결과값이 만약 프로미스라면 아래 if문 진행
				if (acc instanceof Promise) return acc.then(recur);
			}
			return acc;
	});
});

go(Promise.resolve(1),
	a => a + 10,
	a => Promise.resolve(a + 100)
	a => a + 1000,
	a => a + 10000,
  log);
```

- reject될때 결과

프로미스가 reject되면 다음 인자들이 실행이 되지않고 reject 구간에서 딱 끝나게됩니다.

```jsx
go(Promise.resolve(1),
	a => a + 10,
	a => Promise.reject('error'),
	a => console.log('----'),// 실행되지않음
	a => a + 1000,
	a => a + 10000,
  log);
```

- reject 될때 catch로 잡기

```jsx
go(Promise.resolve(1),
	a => a + 10,
	a => Promise.reject('error'),
	a => console.log('----'),// 실행되지않음
	a => a + 1000,
	a => a + 10000,
  log).catch(a => console.log(a));
```

> 프로미스를 단순히 콜백지옥 해결용도로만 쓰는게 아니라 프로미스라는 값을 가지고 내가 원하는 시점에 원하는 로직을 적절한 시점에 실행하는 고차함수를 만들다거나 이런 응용들을 다양하게 할 수 있다.

## promise.then의 중요한 규칙

then 메소드를 통해서 결과를 꺼냈을때의 값이 반드시 프로미스가 아니라는 규칙입니다.

```jsx

// 단 한번의 then으로 결과를 꺼내볼 수 있다.
Promise.resolve(Promise.resolve(Promise.resolve(1))).then(log);// 1

// 프로미스 체인이 연속적으로 대기가 걸려있어도 내가 원하는 곳에서 한번에 then으로 해당하는 결과를 받을 수 있다.
// 그 규칙은 자바스크립트 언어와 개발자가, 개발자와 개발자가, 서로 소통하는데 있어 중요한 법칙이 된다.

// 이 법칙을 가지고 정확히 소통할 수 있다.
// 연속적으로 resolve를 한다고 하더라도 해당하는 결과는 한번에 then으로 꺼내와볼수잇다.
new Promise(resolve => resolve(new Promise(resolve => resolve(1)))).then(log);// 1

```

아무리 프로미스가 중첩된다고 하더라도 내가 원하는곳에서 한번에 then으로 꺼내서 사용할 수 있고

then으로 꺼낸 이 결과가 안쪽에 아무리 깊이 프로미스를 대기를 했어도 프로미스 안쪽에 있는 값을 한번에 꺼낸다는 것이 중요한 규칙중 하나입니다.
