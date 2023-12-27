# Symbol

```javascript
let pKey = Symbol("pKey");
let user = {
    name: '황희정',
    [pKey]: 'password123',
}

for (const key in user) {
    console.log(key);
}
```
> Symbol인 pKey는 보이지않는다.

for-in 루프는 열거 가능한(enumerable) 프로퍼티를 순회한다. Object.getOwnPropertyDescriptors(obj)를 사용하여 확인하면 pKey는 enumerable 속성이 true로 설정되어 있다. <br/>그럼에도 불구하고 Symbol 프로퍼티는 명시적으로 접근해야만 사용할 수 있도록 JavaScript에서 설계되었기 때문에 for-in 루프에서는 보이지 않는다.

```javascript
obj[pKey];// 직접접근
```

따라서 Symbol을 사용하면 유일한 key를 생성할 수 있을 뿐만 아니라, Symbol로 추가한 key를 노출시키지않고 감출 수 있다는 이점이 생긴다.

## Symbol 함수보다 Symbol.for 메서드를 사용하면 전역 심벌 레지스트리를 통해 공유할 수 있다.
- Symbol 함수는 전역 심벌 레지스트리에 관리되지않는다. (전역 심벌 레지스트리에서 심벌 값을 검색할 수 있는 키를 지정할 수 없으므로)
- Symbol.for 메서드를 사용하면 애플리케이션 전역에서 중복되지 않는 유일무이한 상수인 심벌 값을 단 하나만 생성하여 전역 심벌 레지스트리를 통해 공유할 수 있다.

### 동작방식
Symbol.for("키") 메서드를 사용하여 검색에 성공하면 새로운 심벌값을 생성하지 않고 검색된 심벌 값을 반환한다.
검색에 실패하면 새로운 심벌값을 생성하여 키로 전역 심벌 레지스트리에 저장한 후, 생성된 심벌 값을 반환한다.

```javascript
// 심벌값 description이 같더라도 유일무이한 심벌값을 생성한다
const m1 = Symbol('mySymbol')
const m2 = Symbol('mySymbol')
m1 === m2;// false

// Symbol.for메서드는 인수로 전달된 키가 전역 심벌 레지스트리에 검색되면 검색된 심벌값을 반환한다.
const s1 = Symbol.for('mySymbol');
const s2 = Symbol.for('mySymbol');
s1 === s2 // true
```

## Symbol.keyFor 메서드
전역 심벌 레지스트리에 저장된 심벌값의 키를 추출
```javascript
const s1 = Symbol.for('mySymbol')
Symbol.keyFor(s1);// mySymbol
```