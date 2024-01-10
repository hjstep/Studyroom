# useLayoutEffect 훅
- 함수의 시그니처가 useEffect와 동일하지만, useEffect와는 콜백함수 실행 시점이 다르다.
    - **useLayoutEffect는 브라우저에 변경사항이 반영되기전에 실행되는 반면 useEffect는 브라우저에 변경사항이 반영된 이후에 실행되기 때문**
- 컴포넌트 렌더링(컴포넌트 DOM 업데이트) 후에 useLayoutEffect의 콜백 함수 실행이 **동기적으로** 발생한다.

- `실행 순서`
    1. 리액트가 DOM 업데이트
    2. **useLayoutEffect를 실행**
    3. 브라우저에 변경 사항을 반영
    4. useEffect를 실행
- useLayoutEffect는 **동기적으로 실행**
    - 리액트는 useLayoutEffect의 실행이 종료될 때까지 기다린 다음에, 화면을 그린다.
    - 잠시동안 일시중지 발생 (성능 주의)
- `용도`
    - DOM은 계산됐지만 이것이 화면에 반영되기전에 하고싶은 작업이 있을때
    - 애니메이션, 스크롤 위치 제어 등 화면에 반영되기 전에 하고싶은 작업을 하면 훨씬 더 좋은 UX를 제공할 수 있다(잘사용해야함)