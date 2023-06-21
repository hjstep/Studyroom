# 구현 (Implementation Algorithm)
> 이코테 책

- 구현이란 머릿속에 있는 알고리즘을 소스코드로 바꾸는 과정이다.
- 구현 문제 유형은 모든 범위의 코딩 테스트 문제 유형을 포함하는 개념이다.

## 예제 4-1) 상하좌우
```javascript
function upDownLeftRight(n, planning) {
    let x=y=1;
    planning = planning.split(' ');
    for (let p=0; p < planning.length; p++) {
        if (planning[p] === 'L' && x-1 > 0) {
            --y
        } else if (planning[p] === 'R' && y+1 <= n){
            ++y
        } else if (planning[p] === 'U' && x-1 > 0) {
            --x
        } else if (planning[p] === 'D' && x+1 <= n) {
            ++x
        }
    }
    return `${x} ${y}`
}

upDownLeftRight(5, 'R R R U D D')// 3 4
```

## 예제 4-2) 시각
```javascript
function timeSearchFor3(n) {
    let count=0;
    for (let i=0; i<n+1; i++) {
        for (let h=0; h<60; h++) {
            for (let m=0; m<60; m++) {
                const time = i+'' + h +''+ m + '';
                if (time.includes('3')) count++;

            }
        }
    }
    return count;
}

timeSearchFor3(5);// 11475
```

## 실전문제) 왕실의 나이트
```javascript
function night(position) {
    let x = ['a','b','c','d','e','f','g','h'];
    let y = ['1','2','3','4','5','6','7','8']
    let xindex = x.indexOf(position[0]);
    let yindex = y.indexOf(position[1]);
    let count = 0;

    let move = [[2, 1], [2, -1], [-2, -1], [-2, 1], [1, 2], [1, -2], [-1, 2], [-1, -2]];
    
    for (let i=0; i<move.length; i++) {
        if (x[xindex+move[i][0]] && y[yindex+move[i][1]]) {
            console.log(x[xindex+move[i][0]], y[yindex+move[i][1]]);
            count++;
        }
    }
    return count;

}
night('c2');
```


## 실전문제) 게임 개발
```javascript
function character(n,m, a,b,d, mapInfo) {
    
    // 북0 동1 남2 서3
    let move = [[-1, 0], [0, 1], [1, 0], [0, -1]];
    let x = a;
    let y = b;
    mapInfo[x][y] = 1;
    let nowD = d;
    let result = 1; // 방문한칸
    while (true) {
        if (nowD > 3) nowD = 0;
        let flag = false;
        for (let i=nowD; i < nowD+4; i++) {
            let moveType = move[i%4];
            if (mapInfo[x+moveType[0]][y+moveType[1]]) continue;
             mapInfo[x+moveType[0]][y+moveType[1]] = 1;
             x = x+moveType[0]
             y = y+moveType[1]
             flag = true;
             nowD = i%4;
             break;
        }
        if(!flag) break;
        result++;
    }
    return result;
    
}

character(4,4, 1,1,0, [[1,1,1,1],[1,0,0,1],[1,1,0,1],[1,1,1,1]])
```
