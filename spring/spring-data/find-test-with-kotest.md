# find-test-with-kotest

Spring Data JPA를 사용하면 findBy- 라는 메소드를 사용할 수 있다.

JpaRepository 를 상속받게 되면 Pageable 을 이용하여 페이징 처리도 간편하게 할 수 있다.

하지만 kotlin + kotest 를 이용하여 테스트코드를 작성한다면 당황스러운 일을 겪을 수 있다.

```kotlin
every { repository.findAll(any()) } returns PageImpl(mutableListOf(), PageRequest.of(0, 20), 100)
```

위와같이 테스트코드를 작성하면 당연히 될거라고 생각하겠지만 아래와 같은 에러가 발생한다.

![find-test-with-kotest%2048b42ba5c99348fe89f6cdd46e508ca2/Untitled.png](find-test-with-kotest%2048b42ba5c99348fe89f6cdd46e508ca2/Untitled.png)

무슨에러인가 보았더니 Overload 된 메소드여서 발생하는 오류였다.

아래와 같이 매개변수가 1개인 동일한 이름의 메소드가 여러개 있어서 어떤 메소드를 이용해야 할지 모른다는 것이다.

```kotlin
fun findAll(arg: String)
fun findAll(arg: Int)
fun findAll(pageable: Pageable)
```

`any() as 변경할 클래스` 같은 형태로 변경하면 정상적으로 동작한다.

```kotlin
every { repository.findAll(any() as Pageable ) } returns PageImpl(mutableListOf(), PageRequest.of(0, 20), 100)
```

![find-test-with-kotest%2048b42ba5c99348fe89f6cdd46e508ca2/Untitled%201.png](find-test-with-kotest%2048b42ba5c99348fe89f6cdd46e508ca2/Untitled%201.png)