# use-enum-as-request-param

Controller 에서 파라미터로 받아 Enum 으로 변환해 사용하는 요소들이 존재한다.

수동으로 변환하는 수고를 덜고자 Enum 으로 파라미터로 받는방법을 소개한다.


아래와 같이 Enum Class를 선언합니다.

```kotlin
enum class OrderType(
    @JsonValue val code: String
) {
    CREATED_AT("0"),
    BUDGET("1");

    companion object {
        private val map = values()
            .associateBy(OrderType::code)

        fun findBy(code: String?): OrderType? {
            return if(code == null) {
                null
            } else {
                map[code]
            }
        }
    }
}
```


Converter를 만들고 Web Config 에 Formatter 로 추가 해줍니다.

```kotlin
@Configuration
class WebConfig : WebMvcConfigurer {

    override fun addFormatters(registry: FormatterRegistry) {
        registry.addConverter(OrderTypeConvert())
    }
}

class OrderTypeConvert : Converter<String?, OrderType> {
    override fun convert(source: String): OrderType? {
        return OrderType.findBy(source)
    }
}
```


Controller 에서는 아래와 같이 사용합니다.

```kotlin
@GetMapping
fun get(
    @RequestParam("orderType") orderType: OrderType?
): BaseResponse<String> {
    return BaseResponse("success")
}
```