# nested-objectId-serialize-deserialize

ObjectId 를 프로퍼티로 가지고 있는 class 를 serialize 한 후 deserialize 하면 값이 달라지는 현상이 일어난다.

```kotlin
class RedisSerializerTest {

    @Test
    fun test() {
				val objectMapper = ObjectMapper()
        val serializer = Jackson2JsonRedisSerializer(ErrorExample::class.java)
            .also { it.setObjectMapper(objectMapper) }

        val id = ObjectId("6239341381210e5903548df7")

        val actual = ErrorExample(id)
        val serialize = serializer.serialize(actual)
        val deserialize = serializer.deserialize(serialize)
        // fail
        assertEquals(actual, deserialize)
    }
}

data class ErrorExample(
    val id: ObjectId
)
```

![Untitled](nested-objectId-serialize-deserialize%209a95202d8ac046e1b205a9ddfc8eeedb/Untitled.png)

ObjectId 를 serialize 하게 되면 아래와 같은 형태를 가지게 된다.

```kotlin
{ "timestamp": , "counter": , "randomValue1": , "randomValue2":  }
```

이 문제를 해결하기 위해서는 ObjectId 를 serialize 할때 String 으로 변환하고 deserialize 할때 변환된 String 으로 ObjectId 를 생성하도록 하면된다.

방법은 아래와 같다.

 1. Serializer 생성

```kotlin
class ObjectId2StringSerializer : JsonSerializer<ObjectId?>() {

    override fun serialize(value: ObjectId?, jgen: JsonGenerator, provider: SerializerProvider?) {
        jgen.writeString(value.toString())
    }
}
```

1. ObjectMapper 에 Module 등록

```kotlin
fun objectMapper(): ObjectMapper {
		val objectMapper = ObjectMapper()
		objectMapper.registerModule(objectId2StringModule())
}

private fun objectId2StringModule(): SimpleModule {
    return SimpleModule("ObjectId2StringModule").also { it.addSerializer(ObjectId::class.java, ObjectId2StringSerializer()) }
}
```

![Untitled](nested-objectId-serialize-deserialize%209a95202d8ac046e1b205a9ddfc8eeedb/Untitled%201.png)