# 재귀용법

재귀용법(recursive call, 재귀호출)은 함수안에서 동일한 함수를 호출하는 형태이다.<br/>
그리고 재귀호출은 스택의 전형적인 예이다.

> 함수 안에서 동일한 함수를 호출하는 형태

- 예제 : 팩토리얼
```javascript
function 팩토리얼(num) {
	if (num > 1) {
		return num * 팩토리얼(num - 1)
	} else {
		return 1;
	}
}
```

## 시간 복잡도와 공간 복잡도
- factorial(n) 은 n - 1 번의 factorial() 함수를 호출해서, 곱셈을 함
    - 일종의 n-1번 반복문을 호출한 것과 동일
    - factorial() 함수를 호출할 때마다, 지역변수 n 이 생성됨
- 시간 복잡도/공간 복잡도는 O(n-1) 이므로 결국, 둘 다 O(n)
