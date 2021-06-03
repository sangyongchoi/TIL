# kotest-configuration

요즘 Kotlin + Spring Boot 조합이 꽤 뜨고있다.

Kotlin 환경에서 테스트를 손 쉽게 할 수 있는 kotest 라는 것이 존재한다.

지금부터 kotest 설정 방법을 알아보도록 하겠다.

먼저 아래와 같은 의존성을 추가한다.

```yaml
testImplementation("org.springframework.boot:spring-boot-starter-test") { 
	exclude(group = "org.junit.vintage", module = "junit-vintage-engine")
}
testImplementation("io.kotest:kotest-runner-junit5:4.3.0") 
testImplementation("io.kotest:kotest-assertions-core:4.3.0") 
testImplementation("io.kotest:kotest-extensions-spring:4.3.0") 
testImplementation("io.mockk:mockk:1.10.2")
testImplementation("com.ninja-squad:springmockk:2.0.3")
```

추가로 test 폴더에 아래와 같은 설정을 추가해준다.

```kotlin
@Suppress("Unused")
object ProjectConfiguration : AbstractProjectConfig() {

    override fun listeners(): List<Listener> {
        return listOf(SpringListener)
    }
}
```