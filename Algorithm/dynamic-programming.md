# 동적계획법 (Dynamic Programming)

## 정의

- 동적계획법 (DP 라고 많이 부름)
    - 입력 크기가 작은 부분 문제들을 해결한 후, 해당 부분 문제의 답을 활용해서, 보다 큰 크기의 부분 문제를 해결, 최종적으로 전체 문제를 해결하는 알고리즘
    - **상향식 접근법**으로, **가장 최하위 해답을 구한 후, 이를 저장하고, 해당 결과값을 이용해서 상위 문제를 풀어가는 방식**
    - **Memoization 기법을 사용함**
        - Memoization (메모이제이션) 이란: **프로그램 실행 시 이전에 계산한 값을 저장하여, 다시 계산하지 않도록 하여 전체 실행 속도를 빠르게 하는 기술**
    - **문제를 잘게 쪼갤 때, 부분 문제는 중복되어, 재활용됨**
        - 예: 피보나치 수열

## 피보나치수열 예제

- 재귀용법 활용
```javascript
const dp = [0];

function fibonacci(num) {
  if (num <= 2) return 1;
  if (dp[num]) return dp[num];
  dp[num] = fibonacci(num - 1) + fibonacci(num - 2);
  return dp[num];
}
```

- 동적계획법 활용
```javascript
const dp = [0, 1, 1];

function fibonacci(num) {
  for (let i = 3; i <= num; i++) {
    dp[i] = dp[i - 1] + dp[i - 2];
  }
  return dp[num];
}
```
한번 계산된 값은 배열에 저장이되서 그 값을 사용할때는 피보나치수열 값을 추가적으로 다시 계산할 필요없이
저장된 값을 바로 사용한다.

return dp[num]을 하면 피보나치수열값이 계산이 되어있으니 바로 리턴함.

## Refer
> https://mnxmnz.github.io/computer-science/dynamic-programming-in-javascript/
