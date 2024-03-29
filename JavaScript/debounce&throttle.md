# 디바운스와 스로틀
디바운스와 스로틀은 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서
과도한 이벤트 핸들러의 호출을 방지하는 프로그래밍 기법이다.
> 타이머 함수가 사용됨

## 디바운스 debounce
> delay 시간동안 이벤트가 발생하지 않을 때 호출되도록

디바운스는 짧은 시간 간격으로 이벤트가 연속해서 발생하면
이벤트 핸들러를 호출하지 않다가
일정 시간이 경과한(delay) 이후에
이벤트 핸들러가 1번만 호출되도록 한다

- debounce 함수가 반환하는 클로저가 이벤트 핸들러로 등록된다.
(클로저는 timerId를 참조하고있다)

## 스로틀 throttle
> delay 시간 간격으로 콜백 함수가 호출된다.

스로틀은 짧은 시간 간격으로 이벤트가 연속해서 발생하더라도
일정 시간 간격으로 이벤트 핸들러가 최대 1번만 호출되도록 한다.

- throttle 함수가 반환하는 클로저가 이벤트 핸들러로 등록된다.
(클로저는 timerId를 참조하고있다)