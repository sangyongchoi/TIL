# use-in-query

JPA 에서 In 쿼리를 사용하기 위하여 아래와 같은 코드를 작성하였다.

```kotlin
interface BoardReplyRepository : JpaRepository<BoardReply, Long>{
    fun findAllByBoard(boards: List<Board>): List<BoardReply>
}
```

그랬더니 Operator SIMPLE_PROPERTY on board requires a scalar argument, found interface java.util.List in method public abstract java.util.List com.board.dev.domain.board.domain.BoardReplyRepository.findAllByBoard(java.util.List). 이러한 에러가 발생했다.

원인은 쿼리 이름이 ByBoard (단일) 인데 인자로 복수의 타입이 들어와서 그렇다.

메소드를 아래와 같이 변경해주면 정상적으로 작동한다.

```kotlin
interface BoardReplyRepository : JpaRepository<BoardReply, Long>{
    fun findAllByBoardIn(boards: List<Board>): List<BoardReply>
}
```