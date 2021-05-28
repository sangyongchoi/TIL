# Coroutine-context-and-dispatchers

`Coroutine Context`

- 다양한 요소들의 집합
- 주요 요소는 Job 과 Dispatcher

`Coroutine Dispatcher`

- Coroutine 이 실행하기 위해 사용하는 Thread 를 결정
- Coroutine 실행을 특정 `Thread` 로 제한하거나 `Thread pool` 로 디스패치하거나 제한없이 실행되도록 할 수 있다.

출처 : [https://kotlinlang.org/docs/coroutine-context-and-dispatchers.html](https://kotlinlang.org/docs/coroutine-context-and-dispatchers.html)