# smart-cast

Java 에서는 다른 타입으로 변환을 해야할 때 지정한 타입으로 변환이 불가능한 객체라면 Exception 이 발생한다.

그렇기 때문에 instanceof 를 활용하여 변환이 가능한지 아닌지 검사를 해야 한다.

코드는 아래와 같다.

```java
Object data = "Hello world";
int length = -1;

if(data instanceof String) {
	length = (String)data.length;
}
```

위와 같이 작성하면 코드가 간결하지가 않다.

kotlin 에서는 타입변환을 위해 지원하는 Smart Cast 라는 특별한 기법이 있다.

```kotlin
fun `스마트 캐스트 타입이 맞을 때`() {
	val payload: Any = "Hello World"

	val length: Int = if (payload is String) {
				payload.length
			} else {
        -1
      }

	println(length)
}
```

위와같이 작성을 하게 되면 payload 가 String 타입이 맞다면 자동으로 변환되서 캐스팅연산자 필요없이 payload.length 와 같은 형태로 작성할 수 있다.

또한 String타입이 아닐 때 기본값을 지정해줄 수 있다.

위의 코드에서 length의 값은 11 이 출력된다.

```kotlin
@Test
fun `스마트 캐스트 else`() {
	val payload: Any = 1

	val length: Int = if (payload is String) {
		payload.length
	} else {
		-1
	}

	println(length)
}
```

위의 코드는 String 타입이 아니므로 기본값인 -1이 출력된다.