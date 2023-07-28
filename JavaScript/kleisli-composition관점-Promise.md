# Kleisli Composition 관점에서의 Promise

> 함수형 강의 섹션8 내용 정리 <br/>
> (강의명: 함수형 프로그래밍과 JavaScript ES6+ 유인동 강사님)

Promise는 Kleisli Composition을 지원하는 도구라고 볼 수 있다.<br/>
Kleisli arrow 함수 합성방법은 오류가 있을 수 있는 상황에서의 함수 합성을 안전하게 하는 하나의 규칙이다.<br/>
상태, 효과, 외부세상에 의존을 안할수가 없기에 함수 합성이 원하는대로 되지않을 수 있다.<br/>
에러가 발생할 수 있는 상황을 해결하기 위한 함수합성 방법이다.

```javascript
// f . g // f와 g를 함수 합성했다고 했을떄
// 양 변에 x가 같다면 결과가 같다라는 법칙
// f(g(x)) = f(g(x)) // x 인자가 전달되었을때의 결과는 어느시점에 하더라도 항상 동일해야하기 때문에 이 식이 성립되어야함
```

하지만 실무에서는 좌변 f(g(x))를 평가할때 g가 바라보고 있는 값이 우변 f(g(x))를 평가할때는 다른값으로 변해있다거나 값이 없어지거나 하는 상황으로 인해 오류가 발생할 수 있고 오류가 발생했을 때는 f(g(x)) = f(g(x))가 성립되지 않기 때문에 사실상 순수한 함수형 프로그래밍을 할수가없다.<br/>
—> 그런 상황에서도 특정한 규칙을 만들어 합성을 안전하게 만들고 이것을 조금더 수학적이라고 바라볼 수 있게 만드는 함수 합성법이 Kleisli Composition 이다.<br/>

- Kleisli Composition 규칙 - g에서 에러가 난 경우

```javascript
`f(g(x))` 이렇게 실행했을때 `g(x)`x를 g에 전달했을때 에러가발생하면 f(g(x))와 g(x)의 결과가 같다고 볼수 있다.
// f(g(x)) = g(x) // 같은 결과를 만드는 식으로 합성되는 것을  Kleisli Composition라고 한다.
```

g에서 바라보는 값이 잘못되거나 x의 상태에 따라 에러가 발생했을 때 원하지않는 결과를 만들게 되는데<br/>
g(x)가 리턴한 값이 f(g(x))처럼 f를 합성하지않아도 양변이 동일한 값이 만들어지는 합성이다.

- 예시

```javascript
var users = [
	{ id: 1, name: 'aa' },
	{ id: 2, name: 'bb' },
	{ id: 3, name: 'cc' },
];

//유저를 id값으로 찾는 함수
const getUserById = id => find(u => u.id == id, users);

// name을 추출하는 함수
const f = ({name}) => name;
const g = getUserById;

// g를 통해 상태를 찾고 찾은 결과에 f를 연속적으로 실행해서 그사람의 이름을 추출하는 합성을 하고자한다면 
// 함수합성이 된 fg는 아래와같이 만들수 있다.
const fg = id => f(g(id))

fg(2);// g에서 { id: 2, name: 'bb' },이 객체를 찾아내고 그다음에 f에서 name을 구조분해하여 꺼내 name을 전달하게됨

// 2라는 값이 잘 전달된다면 문제없이 동작할것
log(fg(2));

// 항상 동일한 값을 잘만들 수 있다는 것
log(fg(2) == fg(2));
```

- 실무에서의 프로그래밍

```javascript
// 실무에서는 users의 상태가 변하기도한다.

// 예를들어 아래처럼 2번의 pop이 발생해서 user의 상태를 본다면
//users.pop();
//users.pop();
//log(users); // [{id:1, name: 'aa'}] 이런식으로 상태가 변해있고 2번의 유저가 사라져있음

// 정상 상황
const result = fg(2);

users.pop();
users.pop();

// 에러 발생
const result2 = fg(2); 

```

const getUserById = id => find(u => u.id == id, users);<br/>
const f = ({name}) => name;<br/>
const g = getUserById;<br/>
위 코드 자체는 안전하게 코드가 동작하는 함수들이지만 함수 두개(f와 g)를 합성한 상황에서

`const fg = id => f(g(id))` 위험한 상황이 생기게된다.

- 왜 위험한 상황이 생기는가?

1) f라는 함수는 항상 반드시 name을 가진 객체를 인자로 받았을 때만 정상동작하고요

2) g라는 함수는 users안에 결과가 있다고 가정했을때 정상적으로 동작하는 함수가됩니다

만약 pop이 일어나지않았고 fg에 users에 존재하는 id만 전달된다면 문제없이 동작되는데 pop과같은 외부의 변화가 일어난다면 상황에 따라 에러가 발생할 수 있기 때문에 함수를 합성하면서 에러가 나지 않게 하는것이 Kleisli arrow라고 볼 수 있다.

## Kleisli arrow로 합성해보기

```javascript
const fg = id => Promise.resolve(id).then(g).then(f);

fg(2).then(log); // fg(2) 실행결과를 log로 출력해보기 // bb

```

- 외부세상 변화 줘보기

```javascript
users.pop();
users.pop();

fg(2).then(log);// 에러발생
```

- 에러발생되는 상황에서 Promise는 함수 합성을 안전하게 만들 수 있다.

```javascript
// 결과가 없을때 Promise.reject이라는 값을 리턴하게 한다.
const getUserById = id => find(u => u.id == id, users) || Promise.reject('없음!');

users.pop();
users.pop();

g(1) // 정상
g(2) // rejected된 프로미스 값을 받고있음
```

- 결국 g함수는

g(2)는 어떤 상황에서는 똑같은 g(2)를 호출해도 엉뚱한 결과를 만들 수 있게 된 상황<br/>
이렇게 코딩해놓고 나서 fg(2).then(log); 똑같이 실행하면 아까와는 다르게 rejected된 프로미스가 출력되게된다.

- 마치 함수 합성이 g만 일어난것처럼

fg(2).then(log); 실행결과는 Promise를 그대로 리턴하고 함수합성이 마치 g만 실행한것과 동일한 상황으로 이루어진다.

- 비교

```javascript
f(g(2)); // undefined라는 엉뚱한 결과가 나옴 // 프로미스에 name을 찾으니 undefined

fg(2).then(log);// 엉뚱한 결과를 받아들이지않고 log도 출력되지않는다.
```

- catch로 안전하게!

```javascript
const fg = id => Promise.resolve(id).then(g).then(f).catch(a => a); // reject되면 catch로 안전하게 감

fg(2).then(log);
```

g(2)를 실행하면 없다라는 값이 나오게 되고<br/>
합성된 함수 fg(2).then(log);를 실행하게되면 “없어요!”가 떨어지게된다.<br/>
함수 합성 과정에서 에러가 발생한다면(ex. g에서 에러발생) 다음과정을 실행안하고(ex. f실행안함) catch가 실행되고 그다음 .then(log)로 흘러갈 수 있게 해준다.

> Promise를 통해  Kleisli 관점을 바라볼 수 있다.
>

- Kleisli 관점

```javascript
// f(g(x)) = f(g(x))
// f(g(x)) = g(x)
```
