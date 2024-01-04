# Object.is
- `Object.is` 메서드
    - 일치 비교 연산자(===)보다 예측가능한 정확한 비교결과를 반환함

```javascript
-0 === +0; // true
Object.is(-0, +0); // false

NaN === NaN; // false
Object.is(NaN, NaN); // true
```

## Object.is는 참조가 다른 객체에 대해 비교가 불가능하다
```javascript
Object.is({ hello: 'world' }, { hello: 'world' }) // false
```