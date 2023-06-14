# 그리디 알고리즘 (Greedy Algorithm)
> 이코테 책


- 탐욕법
- 현재 상황에서 지금 당장 좋은 것만 고르는 방법
- 매 순간 가장 좋아 보이는 것을 선택하며, 현재 선택이 나중에 미칠 영향에 대해서는 고려하지 않는다.


## 큰 수의 법칙
```javascript
function lawOflargeNumber (n, m, k) {
    let sortArr = n.sort(function(a, b)  {
      return b - a;
    });
    let firstMaxValue = sortArr[0];
    let secondMaxValue = sortArr[1];
    let sum = 0;

    while (m > 0) {
        for (let i=0; i < k; i++) {
            if (m === 0) break;
            sum = sum + firstMaxValue;
            m--;
        }
        if (m === 0) break;
        sum = sum + secondMaxValue;
        m--;
    }
    return sum;
}

lawOflargeNumber([2,4,5,4,6], 8, 3)
```

## 숫자 카드 게임
```javascript
function cardGame(inputArr) {
    let low = 0;
    for (let i=0; i<inputArr.length; i++) {
				const minValue = inputArr[i].sort(function(a, b) {
				  return a - b;
				})[0];

        if (low === 0) {
            low = minValue;

        } else if (low < minValue) {
            low = minValue;
        }
    }
    return low;
}
let inputArr = [[3,1,2],[4,1,4],[2,2,2]];
cardGame(inputArr);
```

## 1이 될때 까지
```javascript
function untilOne(n, k) {
    let result = n;
    let count = 0;

    while (result > 1) {
        if (result % k === 0) {
            result = result / k;
        } else {
            result = result - 1;
        }
        count++;
    }
    return count
}
untilOne(25,5)
```
