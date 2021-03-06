# 2021-05-13

`Coroutines`

코루틴은 kotlin 을 활용하여 동시성 프로그래밍을 쉽게 할 수 있게 도와주는 친구이다.

오늘은 async, await 에 대해서만 간단하게 정리를 해보겠다.

`async` : 새로운 코루틴을 생성하고 Deferred 객체를 반환합니다.

`await` : Deferred 객체의 결고값을 낼 때까지 기다린다.

```kotlin
internal class TestClassTest {

    suspend fun fun1(): Int {
        delay(1000)
        return 1
    }

    suspend fun fun2(): Int {
        delay(2000)
        return 2
    }

    suspend fun fun3(): Int {
        delay(3000)
        return 3
    }

    @Test
    fun test_case_01() {
        runBlocking {
            println(LocalDateTime.now().toString() + " start")
            val v1 = fun1()
            val v2 = fun2()
            val v3 = fun3()

            println("value : " + (v1 + v2 + v3))
            println(LocalDateTime.now().toString() + " end")
        }
    }

    @Test
    fun async_test_case_02() {
        runBlocking {
            println(LocalDateTime.now().toString() + " start")
            val v1 = async { fun1() }
            val v2 = async { fun2() }
            val v3 = async { fun3() }

            println("value : " + (v1.await() + v2.await() + v3.await()))
            println(LocalDateTime.now().toString() + " end")
        }
    }

    @Test
    fun async_sequential_test_case_03() {
        runBlocking {
            println(LocalDateTime.now().toString() + " start")
            val v1 = async { fun1() }
            val v2 = async { fun2() }
            println("value : " + (v1.await() + v2.await()))
            val v3 = fun3()
            println(LocalDateTime.now().toString() + " end")
        }
    }
}
```

case 01

- async가 걸려있지 않으므로 순차 실행, 6초가 걸립니다.

case 02

- 모두 async가 걸려있으므로 동시실행한다. 3초가 걸립니다.

case 03

- 두개가 async가 걸려있고 하나는 안걸려있는 상황
- 두개중 2초가 제일 오래걸리므로 두개 모두 실행하는데 2초 + 걸려있지 않은 메소드 실행시간 3초    
총 5초가 걸립니다.
