# useInsertionEffect 훅
- 리플로우와 리페인트 전에 호출됨
- CSS-in-JS 라이브러리를 위한 훅 (라이브러리를 구축하지않는이상 실제 앱에서는 사용될일 없음)
- 이 훅 내부에 스타일을 삽입하는 코드를 집어넣음으로써 브라우저가 레이아웃을 계산하기전에 실행될 수 있게 한다. 

### 장점
- 브라우저가 다시 스타일을 입혀 DOM을 재계산하지않아도 된다.

### 호출 순서
> **useInsertionEffect → useLayoutEffect → useEffect**