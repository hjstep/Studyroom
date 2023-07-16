# filter와 map

> 함수형 강의 섹션7 내용 정리 - 2<br/>
> (강의명: 함수형 프로그래밍과 JavaScript ES6+ 유인동 강사님)

## map 함수
L.map + take로 map 만들기

````javascript
L.map = curry(function* (f, iter) {
    for (const a of iter) {
      yield f(a);
    }
  });

// L.map + take로 map 만들기
const map = curry(pipe(L.map, take(Infinity)));

log(map(a => a + 10, L.range(4)));
````

## filter 함수
L.filter 사용한다음 take(Infinity)로 모든 결과를 꺼낸다.

```javascript
L.filter = curry(function *(f, iter) {
	iter = iter[Symbol.iterator]();
	let cur; 
	while (!(cur = iter.next()).done) {
		const a = cur.value;
		if (f(a)) {
			yield a;
		}
	}
});
// L.filter + take로 filter 만들기
const filter = curry(pipe(L.filter, take(Infinity)));
```
