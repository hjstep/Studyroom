# 리액트 파이버
React 16에 등장한 가상 DOM을 이용하여 렌더링 과정을 최적화하기 위한 아키텍처다.

- 비동기적으로 렌더링 작업 처리 가능 
  - 렌더링 작업을 일시중단/다시 시작/폐기 가능
- Incremental Rendering(단계적 렌더링)
  - 렌더링 작업을 작은 단위로 쪼개어 분할할 수 있다.
- 우선순위
  - 렌더링 작업에 우선순위를 매길 수 있다.

## 파이버 작업
트리 업데이트 과정을 파이버 단위로 나눠서 수행한다.

1. 변경 감지 : 컴포넌트의 props나 state 변경 감지
2. WorkInProgress 트리 생성 : current 트리를 기반으로 생성한다 (파이버노드 재사용)
3. WorkInProgress 트리에 업데이트 적용 : 업데이트 사항이 WorkInProgress 트리의 각 파이버노드에 적용됨
4. Diffing 알고리즘 적용 : 실제 DOM에 필요한 변경사항을 최소화하기 위해 Diffing 알고리즘을 적용하여 current와 workInProgress를 비교한다.
5. 커밋 단계 : 더블 버퍼링 메커니즘으로, current 트리를 workInProgress 트리로 교체한다. 그리고 이 트리가 UI에 최종적으로 렌더링되어 반영된다.

## 파이버 (FiberNode 생성자함수에 의해 생성된 인스턴스)
- 하나의 element 마다 생성됨
- 파이버간의 관계는 child, sibling, return, index 프로퍼티로 표현
- alternate : 반대편 트리 파이버를 가리킴

## 파이버 트리 2개
1. current 트리 : 현재 화면에 렌더링된 UI 트리
2. workInProgress 트리 : 업데이트가 진행중인 상태를 나타내는 트리