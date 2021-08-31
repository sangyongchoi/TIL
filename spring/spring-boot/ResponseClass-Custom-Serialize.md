# ResponseClass-Custom-Serialize

서비스를 운영하다보면 가끔 Response를 조작해야 하는 경우가 생긴다.

예를들어 사용자의 이름을 익명으로 하기위해 "익명의 사용자" 로 변환을 해야 된다고 가정하자.

```kotlin
data class Response(
	val name: String
)

fun getUsers(): List<Response> {
	val users = userRepository.findAll()
	return users.map { Response("익명의 사용자") }
}
```

위와 같이 코딩을 할 수 있을것이다.

위와같이 코딩을 한다면 아래와 같은 문제점이 존재한다.

1. 너무 많은 로직이 한곳에 몰릴 수 있다. 
name 뿐만 아니라 성별, 나이, 핸드폰번호 등등의 정보가 추가되고 그 데이터도 익명으로 바꿔줘야 한다는 요구사항이 추가된다고 가정해보자.
소스코드가 굉장히 더러워질것이다.
2. 재사용성이 없으며 불편하다.

그렇다면 어떻게하면 조금 더 편리하게 사용할 수 있을까 ?

annotation과 jackson 라이브러리를 이용하여 손쉽게 할 수 있다.

```kotlin
@Target(AnnotationTarget.FIELD)
@Retention(AnnotationRetention.RUNTIME)
@JacksonAnnotationsInside
@JsonSerialize(using = NameMaskSerializer::class)
annotation class NameMasking
```

```kotlin
class NameMaskSerializer: StdSerializer<String>(String::class.java), ContextualSerializer {

    override fun serialize(value: String, gen: JsonGenerator, provider: SerializerProvider) {
        gen.writeString("익명의 사용자")
    }

    override fun createContextual(provider: SerializerProvider, property: BeanProperty): JsonSerializer<*> {
        val annotation = property.getAnnotation(NameMasking::class.java)
        if (annotation != null) {
            return NameMaskSerializer()
        }

        return provider.findKeySerializer(property.type, property)
    }
}
```

```kotlin
data class Response(
	@NameMasking
	val name: String,
)

fun getUsers(): List<Response> {
	val users = userRepository.findAll()
	return users.map { Response(it.name) }
}
```

위와같이 Custom Annotation 을 만든후에 Response Class 의 field에 어노테이션을 붙여주면 된다.

@NameMasking 을 붙여주게 되면 ObjectMapper가 Serialize 할때  @NameMasking위에 붙어있는 @JsonSerialize(using = NameMaskSerializer::class) 에 의해 NameMaskSerializer.serialize 가 실행되면서 "익명의 사용자" 로 변환되게 된다.

Custom Annotation 을 이용하면 변환하는 로직이 Serializer 로 이동하게 되서 비즈니스로직이 굉장히 간편 해진다. 또한 해당 기능이 필요한 field 위에 어노테이션만 붙이면 되니 재사용성과 편의성 또한 좋아진다.