# flatten과 flatMap

> 함수형 강의 섹션7 내용 정리 - 3<br/>
> (강의명: 함수형 프로그래밍과 JavaScript ES6+ 유인동 강사님)

## flatten 함수
- L.flatten 함수: 동적으로 한번에 펼쳐서 한 뎁스를 가진 일정한 배열로 변형해주는 이터레이터를 리턴하는 함수
- flatten 함수: 즉시 모두 평가하는 함수

```javascript
이런 값이 있을때 
[[1,2], 3, 4, [5,6], [7,8,9]]

값을 다 펼쳐서 하나의 배열로 만드는 역할을 한다. 
[...[1,2], 3, 4, ...[5,6], ...[7,8,9]]
// [1,2,3,4,5,6,7,8,9]
```

### L.flatten 함수
flatten의 지연함수
```javascript
const isIterable = a => a && a[Symbol.iterator];

L.flatten = function *() {
    for (const a of iter) {
        if (isIterable(a)) {
            // a안에 있는 값들을 뽑아 모두 yield
            for (const b of a) yield b;
        } else {
            yield a;  
        } 
    }
};

var it = L.flatten([[1, 2], 3, 4, [5, 6], [7, 8, 9]]);
log(it.next());
log(it.next());

// 다뽑음
log([...it]);
```

### flatten 함수
```javascript
const flatten = pipe(L.flatten, takeAll);
log(flatten( [[1,2],3,4,[5,6],[7,8,9]] ));
```

---
## flatMap 함수
flatMap은 map과 flatten을 동시에 하는 함수

````javascript
// flatMap은 인자에 전달된 함수를 이용해서 배열 원소의 값들에 변화를 줘서 사용할 수 있음
// 안쪽에 있는 배열 원소의 값 하나하나 함수 적용
([[1,2], [3,4], [5,6,7], 8,9, [10]]).flatMap(a => a.map(a => a*a)));
// --> 결국 map을 하고 flatten을 한것과 동일하게 동작
````

### L.flatMap
flatMap의 지연함수

```javascript
L.flatMap = curry(pipe(L.map, L.flatten));
```

### flatMap
즉시 평가한 flatMap

```javascript
const flatMap = pipe(L.flatMap, takeAll);
const flatMap = curry(pipe(L.map, L.flatten, takeAll));
const flatMap = curry(pipe(L.map, flatten)); // flatten에서 모든평가를 완료한 flatMap을 만듦
```

## 응용
```javascript
flatMap(range, map(a => a+1, [1,2,3]));

...L.flatMap(L.range, map(a => a+1, [1,2,3]));

flatMap(L.range, map(a => a+1, [1,2,3]));// 즉시평가되는 flatMap 사용

// 이터레이터로 만든 후 next로 필요할때까지만 순회
var it = L.flatMap(L.range, map(a => a+1, [1,2,3]));
it.next();
it.next();
it.next();

// take로 앞에서 3개만 꺼내서 보겠다
take(3, L.flatMap(L.range, map(a => a+1, [1,2,3])))
```
