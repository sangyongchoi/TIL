# how-to-return-specific-fields-with-MongoOperation

Server 에서 사용하여 Collection의 데이터를 가져올 수 있다.

이때 select 시 발생하는 Network 비용을 줄이는것이 성능상 유리하다.

비용을 줄이는 가장 빠르고 효과적인 방법은 select 하는 field 를 줄이는 것이다.

이번 글에서는 `MongoOperation` 을 이용하여 특정 field 만 가져오는 법을 소개하도록 하겠다.

```kotlin
@Document(collection = "Users")
data class User(
    @Id
    val id: ObjectId? = null,
    val name: String? = null,
		val tel: String = "",
		val address: String = "",
		val age:Int = 0,
)
```

위와 같은 Document 가 존재할 때 name 만 가지고 오고싶다고 가정할 때 아래와 같은 방법으로 가져올 수 있다.

```kotlin
class UserRepositoryCustomImpl(
    private val operations: MongoOperations
) {
    fun findBy(): List<UserDto> {
        val query = Query()

				// 조회하고 싶은 field
        query
            .fields()
            .include("name")

        return operations.find(
            query, 
            UserDto::class.java, // 변환할 Dto Class
            "Users", // collection name
        )
    }
}
```