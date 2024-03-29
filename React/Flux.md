# Flux 패턴
- 단방향 데이터 흐름.
- 2014년 경, Flux 패턴과 함께 이를 기반으로 한 라이브러리 Flux를 소개

```javascript
[ Action ]  -->  [ Dispatcher ]  -->  [ Model ]  --> [ View ]
```

- **액션(action)**
    - 어떤 작업을 처리할 액션
    - 액션 발생시 데이터도 함께 보낼 수 있음
- **디스패처(dispatcher)**
    - 액션을 스토어에 보내는 역할
- **스토어(store)**
    - 상태에 따른 값과 상태를 변경할 수 있는 메서드를 갖고있음
    - 액션 타입에 따라 어떻게 변경할지 정의되어있음
- **뷰(view)**
    - 스토어에 만들어진 데이터를 가져와 화면에 렌더링하는 역할
    - 사용자 인터렉션에 따라 상태를 변경하고자 할때, 뷰에서 액션을 호출한다

- **장점**
    - 데이터의 흐름은 모두 액션이라는 단방향으로 줄어들므로 데이터의 흐름을 추적하기 쉽고 코드를 이해하기 한결 수월해진다.
    - 리액트는 단방향 데이터 바인딩을 기반으로 한 라이브러리였으므로 단방향 흐름의 Flux패턴과 잘맞는다
- **단점**
    - 사용자 인터렉션에 따라 데이터를 갱신하고 화면을 어떻게 업데이트해야하는지도 코드를 작성해야하므로 코드의 양이 많아지고 수고스럽다.