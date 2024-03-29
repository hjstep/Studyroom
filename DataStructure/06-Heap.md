# 힙 (Heap)

## 1. 힙 (Heap) 이란?

힙은 트리를 기반으로 특정한 목적에 맞춰 변형된 자료구조이다.

데이터에서 최대값과 최소값을 빠르게 찾기 위해 고안된 완전 이진 트리

최대값과 최소값을 찾기위해서 단순 배열을 사용하면 시간이 오래걸림
(배열에 있는 모든 데이터를 다 검색해서 그 안에서 최대값과 최소값을 찾게되면 O(n) 시간복잡도만큼 걸림.)

→ 이거보다 더 빠르게 찾을 수 있는 방법 중에 하나가 힙이다.

(힙을 사용하면 O(logn)으로 아주 많은 시간을 단축할 수 있음)

- 완전 이진 트리: 노드를 삽입할 때 최하단 왼쪽 노드부터 차례대로 삽입하는 트리
- 힙을 사용하는 이유:
    - 배열에 데이터를 넣고 최대값과 최소값을 찾으려면 O(n)이 걸림
    - 이에 반해, 힙에 데이터를 넣고 최대값과 최소값을 찾으려면 O(logn)이 걸림

## 2. 힙 (Heap) 구조

최대 힙과 최소 힙으로 분류할 수 있음

- 최대 힙(Max Heap) : 최대값을 구하기 위한 구조
- 최소 힙(Min Heap) : 최소값을 구하기 위한 구조

힙에서 데이터는 항상 루트 노드에서 가져온다.

루트 노드가 최대값 또는 최소값이기 때문에 데이터를 다 탐색할 필요없이 루트 노드만 빼오면 되기 때문에 시간이 적게 걸린다. (최대, 최소값을 항상 마련해놓는 구조가 되니깐)

→ 물론 힙 정책에 맞게 데이터를 구성하는데에는 시간이 걸립니다.

완전 이진 트리

- 데이터를 넣을 때 항상 왼쪽부터 채워가게 되어있는 구조

힙은 두가지 조건을 가지고 있는 자료구조임

1. 각 노드의 값은 해당 노드의 자식 노드 값보다 크거나 같다. (최대 힙인 경우)
    1. 최소 힙의 경우는 각 노드의 값은 해당 노드의 자식 노드가 가진 값보다 크거나 작음
2. 완전 이진 트리 형태를 가짐

## 3. 힙(완전 이진 트리) vs 이진 탐색 트리

- 공통점: 모두 이진 트리이다. (자식노드가 최대 2개인것)
- 차이점
    - 힙(완전 이진 트리)은 각 노드의 값이 자식 노드보다 크거나 같음 (최대 힙인 경우)
    - 이진 탐색 트리는 왼쪽 자식 노드의 값이 가장 작고, 그다음 부모노드, 그다음 오른쪽 자식 노드 값이 가장 큼
    - 힙(완전 이진 트리)은 왼쪽 및 오른쪽 자식 노드의 값은 오른쪽이 클 수도 있고 왼쪽이 클 수도 있음
    - 이진 탐색 트리는 탐색을 빠르게 하기 위한 구조,
    - 힙은 최대/최소값 검색을 빠르게 하기 위한 구조
