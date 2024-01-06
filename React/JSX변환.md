## JSX는 트랜스파일링을 거쳐 ECMAScript로 변환된다

**`React.createElement()` 또는 `_jsxRuntime.jsx()`**
- `인수`
    - 첫번째 인수 : JSXElement로 요소정의
    - 나머지 인수(옵션) : 옵셔널인 JSXChildren, JSXAttributes, JSXStrings를 이후 인수에 넘겨 처리
- `React.createElement()`
    - 이전 버전에 사용함
    - React 정적메서드
- `_jsxRuntime.jsx()`
    - 최신버전(리액트17, 바벨 7.9.0 이후 버전)
    - **트리 쉐이킹과 번들 크기 최적화 기능**
    - `var _jsxRuntime = require('custom-jsx-library/jsx-runtime');`
    - 특이한 점: 인스턴스 메서드로 호출되지않고 **일반 함수로 호출된다.** 따라서 this는 undefined이다. (strict mode 이므로)
