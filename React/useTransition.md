# useTransition 
리액트 18의 핵심인 “**동시성**(concurrency) 렌더링”을 다룰 수 있는 훅이다.
- 무거운 연산 UI 조각을 렌더링 일으키는 상태 업데이트에 대해 낮은 우선순위를 부여하여 상태 변경을 지연시켜서 렌더링을 지연시킨다.

## 특징
- 비동기 렌더링 (지연 렌더링) 처리
- 로딩 상태 관리
- 낮은 렌더링 우선순위 부여

## 사용법
- `const [isPending, startTransition] = useTransition();`
- 훅을 사용할 수 없다면 `import { startTransition } from ‘react’;`로도 사용가능