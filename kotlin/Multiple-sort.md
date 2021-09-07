# Multiple-sort

기능을 구현하다보면 여러가지 조건으로 정렬을 해야할때가 있다.

```kotlin
data class Grade(
	val scroe: Int,
	val time: LocalDateTime,
)
```

예를들어 위와같은 클래스가 있을 때 score 가 높은순으로 스코어가 같다면 time이 높은순으로 정렬을 해야한다는 요구조건이 있다고 가정해보자.

kotlin 에서 손쉽게 할 수 있는 방법을 소개하겠다.

```kotlin
fun main() {
    val now = LocalDateTime.now()
    val a = Grade(10, now.minusDays(10))
    val b = Grade(8, now)
    val c = Grade(10, now.minusDays(8))
    val list = listOf(a, b, c)

    val sorted = list.sortedWith(
        compareByDescending<Grade> { it.score }.thenByDescending { it.time }
    )

    println(sorted)
}

data class Grade(
    val score: Int,
    val time: LocalDateTime,
)
```

score 를 기준으로 1차 sort 한후 time 을 기준으로 2차 sort를 한다.

Descending 이 들어간 메소드는 DESC 정렬이고 Descending 을 뺀 메소드는 ASC 정렬이다.