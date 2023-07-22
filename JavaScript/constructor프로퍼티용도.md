# constructor 프로퍼티 용도

## 1. 인스턴스 생성

constructor 프로퍼티는 기존 객체를 기반으로 새로운 객체를 생성할 때 유용하다.

```javascript
let obj = new obj.constructor();
```

## 2. 객체의 타입 확인 (생성자 함수 식별)

constructor 프로퍼티를 사용하면 객체가 어떤 생성자를 통해 만들어졌는지 확인할 수 있다.

```javascript
function Person(name) {
    this.name = name;
}

const me = new Person('Lee');
console.log(me.constructor === Person); // true
```

## 3. 생성자 함수와 constructor간의 연결이 파괴될때 원래의 생성자를 가리키도록 되살릴때

### 프로토타입 교체
프로토타입을 교체할 때 constructor 프로퍼티를 지정하지않으면 생성자함수와 constructor 프로퍼티간의 연결이 파괴된다. 
따라서 원래의 생성자를 가리키도록 되살려야한다.

```javascript
const Person = (function() {
	function Person(name) {
		this.name = name;
	}

	Person.prototype = {
		constructor: Person, // constructor프로퍼티와 생성자함수간의 연결을 설정
		sayHello() {
			//...
		}
	};

	return Person;
}());

const me = new Person('Lee');

me.constructor === Person // true
me.constructor === Object // false
```

### Object.create 메소드를 이용한 직접 상속
```javascript
function Animal(name) {
    this.name = name;
}

Animal.prototype.breath = function() {
    console.log(`${this.name} is breathing...`);
}

function Dog(name) {
    this.name = name;
}

Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog; // constructor를 다시 설정
```
