# 이진탐색
> 이코테 책

- 반으로 쪼개가며 탐색하기
- 탐색 범위를 절반씩 좁혀가며 데이터를 탐색한다.
- 배열 내부가 정렬이 되어있어야만 가능

## 이진탐색 자바스크립트 구현
```javascript
function bindarySearch(start, end, array, target) {
    while (start <= end) {
        let mid = Math.floor((start+end) / 2);

        if (array[mid] === target) {
            return true
        }

        if (array[mid] > target) {
            end = mid - 1;
        } else {
            start = mid + 1;
        }
        
    }
    return false;
}
```

## 부품찾기 문제

```javascript
function bindarySearch(start, end, array, target) {
    while (start <= end) {
        let mid = Math.floor((start+end) / 2);

        if (array[mid] === target) {
            return true
        }

        if (array[mid] > target) {
            end = mid - 1;
        } else {
            start = mid + 1;
        }
        
    }
    return false;
}

function findFactory(n, totalArray, m, requestArray) {
    let resultArray = [];
    for (let i=0; i < m; i++) {
        let result = bindarySearch(0, n-1, totalArray.sort((a, b) => a - b), requestArray[i])
        resultArray.push(result);
    }

    return resultArray.map((item) => item ? 'yes' : 'no').join(' ');
}

console.log(findFactory(5, [8,3,7,9,2], 3, [5,7,9]));

```
