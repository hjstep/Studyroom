# callback과 Promise

> 함수형 강의 섹션8 내용 정리 <br/>
> (강의명: 함수형 프로그래밍과 JavaScript ES6+ 유인동 강사님)

자바스크립트에서 비동기 동시성 프로그래밍에는 크게 2가지가 있다.

1) callback 패펀
```javascript
function add10(a, callback) {
		// a+10결과를 받아둔 함수callback을 실행해서 그 함수에게 결과를 전달하는 식
    setTimeout(() => callback(a + 10), 100);
  }

// a+10 이후에 할일을 callback함수를 통해 정의
add10(5, res => {
	log(res);
});
```
2) Promise를 기반으로한 Promise 메소드 체인을 통해서 함수를 합성하는 방법 또는 async await
```javascript
function add20(a) {
	// 프로미스가 끝났다는것을 resolve함수로 알려주게된다.
	// add20은 add10과의 큰 차이는 바로 "return"이다
	// add20은 Promise를 만들어서 리턴해주고있다.
  return new Promise(resovle => setTimeout(() => resolve(a+20), 100));
}

add20(5).then(log);
```

- 연속적으로 호출하려고할때 둘의 차이가 드러남
```javascript
add10(5, res => {
	add10(5, res => {
		add10(5, res => {
			log(res);
		});
	});
});	

	
// add10보다 비교적 쉽게 작성
add20(5)
	.then(add20)
  .then(add20)
	.then(add20)
  .then(log)
```
프로미스가 callback 패턴보다 좀 더 합성하기 좋고, 가독성도 좋다<br/>
(callback패턴은 계속 연속적으로 들여쓰기로 들어가야하기에 보기 복잡함)<br/>
—> Promise가 callback 지옥을 then then으로 해결

# 비동기를 값으로 만드는 Promise

callback과 Promise의 가장 큰 차이는
Promise는 then 메소드를 통해 결과를 꺼내보기 보다는 비동기 상황을 일급값으로  다룬다는것이 callback패턴의 큰 차이이다.<br/>
Promise는 Promise라는 클래스를 통해서 인스턴스를 반환함. 그 반환값은 대기/성공/실패를 다루는 일급값으로 이루어져있다.

- 리턴값
```javascript
var a = add10(5, res => {
	add10(5, res => {
		add10(5, res => {
			log(res);
		});
	});
});
	
var b = add20(5)
	.then(add20)
  .then(add20)
	.then(add20)
  .then(log)
```
callback 패턴의 리턴값은 아무것도안찍히는 undefined이고
Promise의 리턴값은 즉시 Promise를 반환함

## 결론
callback은 리턴값은 중요하지않고 setTimeout이 일어난다는 코드적인 상황과 시간이 끝났을때 callback을 실행해주고 있는 이 컨텍스트만 남아있는 상황<br/>
Promise를 사용한 함수같은 경우는 이 코드를 평가했을때 그 자리에 바로 즉시 Promise가 리턴됨.<br/>
Promise가 리턴되기때문에 그 이후에 원하는 일들을 다룰수 있음

```javascript
var c = add20(5, _ => _);

var d = c(5, _ => _);// 또 프로미스 리턴
d.then(log); // 계속해서 then을 통해 후속일을 연결지어할 수 있음
```
**Promise는 비동기 상황이 코드로서 다뤄지는게 아니라 값으로서 다뤄진다**.

값으로 다뤄진다는 것은 일급이란 것이고 변수에 할당할 수 있고 함수에 전달할 수 있고 전달된값을 통해 계속해서 어떤 일을 이어나갈수 있다는게 중요
