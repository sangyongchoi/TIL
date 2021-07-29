# Using-mongodb-type-safe-with-kotlin

Spring data mongo 를 사용하게 되면 아래와 같은 방식으로 쿼리를 작성할 수 있다.

```kotlin
Query()
	.addCriteria(
		Criteria.where("id").isEqualTo(1L)
	)
```

이렇게 코딩하게 하게 된다면 문제점이 무엇일까 ?

크게 2가지로 나누어볼 수 있을 것이다.

1. Type-safe 하지 않다.
만약 id가 Long 타입이 아니라 다른타입이라면 ? 바로 오류로 이어질 것이다.
2. 오타로 인해 오류가 발생할 수 있다.
id를 잘못하여 di 로 적는순간 오류로 이어질 것이다. 
id는 짧기에 이런일이 없겠지만 필드명이 길어지는 순간 위험도는 증가한다.

Kotlin을 이용하여 Spring data mongo 를 이용한다면 이러한 문제점을 해결할 수 있다.

`org.springframework.data.mongodb.core.query.TypedCriteriaExtensions`  파일을 확인해보면 아주 멋진 방법으로 이러한 문제를 해결해 놓았다.

바로 아래와같이 infix fun 을 이용하여 해결했다.

```kotlin
fun <T> KProperty<T>.nin(vararg o: Any): Criteria =
		Criteria(asString(this)).nin(*o)
```

그렇다면 이것을 응용하여 어떻게 Type-safe 하고 오타에 안전하게 개발 할 수 있을까 ?

바로 아래와 같이 작성하면 된다.

```kotlin
Query()
	.addCriteria(
		User::id isEqualTo 1L
	)
```

Spring + MongoDB 의 조합을 이용하고 계시다면 Kotlin 을 도입하는것을 고려해볼만한 이유가 될 것 같다.