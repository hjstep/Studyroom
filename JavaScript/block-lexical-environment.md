# BLOCK Lexical Environment

let, const 키워드로 변수가 선언된 모든 블록문은 블록 레벨 스코프를 생성한다 

## BLOCK Lexical Environment 생성 과정
- 선언적 환경 레코드를 갖는 렉시컬 환경을 새롭게 생성한다.
> 새롭게 생성된 렉시컬 환경으로 기존의 렉시컬 환경을 교체한다
- 외부 렉시컬 환경에 대한 참조는 블록문이 실행되기 이전의 렉시컬 환경을 가리킨다

> `LexicalEnvironment` -> `BLOCK Lexical Environment`
> `BLOCK Lexical Environment`의 `EnvironmentRecord` -> `Declarative Environment Record`
> `BLOCK Lexical Environment`의 `OuterLexicalEnvironmentReference` -> 블록문 이전 렉시컬 환경

