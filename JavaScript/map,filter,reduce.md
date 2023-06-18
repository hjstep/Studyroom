# map, filter, reduce

## 함수형 프로그래밍으로 구현한 map 함수

```javascript
const map = (f, iter) => {
  let res = [];
  for (const a of iter) {
    res.push(f(a));// 추상화(f 함수에 어떤값을 수집할것인지 위임한다)
  }
  return res;
};
```
함수형 프로그래밍으로 구현한 map 함수는
- 보조함수(매개변수)를 통해 이터러블 값 중 특정 값을 수집하겠다고 전달
- map함수는 고차함수이다.
- map함수는 이터러블 프로토콜을 따르고 있기 때문에 다형성이 높다 (이터러블 프로토콜을 준수하는 함수들을 다 사용할 수 있음)

---

## 함수형 프로그래밍으로 구현한 filter 함수

```javascript
const products = [
    {name: '반팔티', price: 15000},
    {name: '긴팔티', price: 20000},
    {name: '핸드폰케이스', price: 15000},
    {name: '후드티', price: 30000},
    {name: '바지', price: 25000}
];

const filter = (f, iter) => {
    let res = [];
    for (const a of iter) {
        // 함수를 인자로 받아서 어떤 조건일때 푸쉬할것인지
        // 보조함수에게 위임을한다. (조건을 적은 함수를 인자로 보내야함)
        if (f(a)) res.push(a);// 조건에 따라 필터
    }
    return res;//부수 효과로 출력을 하는 것이 아니라 함수의 리턴 값으로 출력
};
```
필터는 특정 데이터만 걸러내고 싶을때, 조건을 걸어 걸러내는 함수이다.

---

## 함수형 프로그래밍으로 구현한 reduce 함수

```javascript
const reduce = (f, acc, iter) => {// 보조함수, 누적 시작값, 이터레이터
  if (!iter) {
		/* 이터가 없을 경우에 acc가 이터레이터일 것이다. 
           acc에 심볼 이터레이터를 실행해서 이터러블 값을 이터레이터로 완전히 변환함. */
    iter = acc[Symbol.iterator]();
    acc = iter.next().value;// acc 값은 이 이터레이터 안에 있는 첫 번째 값으로 해줘야 되니까 이터레이터 value를 넣어줌.
  }
  for (const a of iter) {
    acc = f(acc, a);
  }
  return acc;
};
```

- reduce는 값을 추격하는 함수이다. (이터러블 값을 하나의 값으로 추격해 나가는 함수)
- 보조함수를 통해서 어떻게 추격할지를 위임한다.
