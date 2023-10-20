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
