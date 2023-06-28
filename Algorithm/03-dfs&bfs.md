# 탐색 알고리즘 DFS/BFS
> 이코테 책

- DFS(Depth-First Search): 깊이 우선 탐색, 그래프에서 깊은 부분을 우선적으로 탐색하는 알고리즘<br/>
  최대한 멀리 있는 노드를 우선으로 탐색하는 방식으로 동작한다.
- BFS(Breadth First Search): 너비 우선 탐색, 가까운 노드부터 탐색하는 알고리즘이다.(DFS의 반대)
- 인접 행렬(Adjacency Matrix) 방식: 2차원 배열에 각 노드가 연결된 형태를 기록하는 방식이다.
- 인접 리스트: 연결 리스트 자료구조를 이용해 구현한다. 2차원 리스트를 이용하면 된다.

## 실전문제 - 음료수 얼려 먹기
```javascript
function icecream(n,m,graph) {
    const visitQueue = Array.from({length: n}, () => Array.from({length: m}, () => false));

    function bfs(i,j, graph) {
        let queue = [];
        const direction = [[-1,0], [1,0], [0, -1], [0, 1]];
        visitQueue[i][j] = true;
        queue.push([i,j]);    
        while (queue.length) {
            let [x,y] = queue.shift();
            for (let i = 0; i < 4; i++) {
              const calcX = x + direction[i][0];
              const calcY = y + direction[i][1];
        
              if (calcX < 0 || calcX >= n || calcY < 0 || calcY >= m)
                continue;
        
              if (!visitQueue[calcX][calcY] && graph[calcX][calcY] === 0) {
                queue.push([calcX, calcY]);
                visitQueue[calcX][calcY] = true;
              }
            }
            
        }
    }

  let count = 0;

  for (let i = 0; i < n; i++) {
    for (let j = 0; j < m; j++) {
      if (!visitQueue[i][j] && graph[i][j] === 0) {//시작
        bfs(i,j, graph, n,m);
        count++;
      }
    }
  }

  return count;
    
}

console.log(icecream(4,5,[[0,0,1,1,0],[0,0,0,1,1],[1,1,1,1,1],[0,0,0,0,0]]));
console.log(icecream(3,3,[[1,1,1],[0,1,0],[1,1,1]]));
```
