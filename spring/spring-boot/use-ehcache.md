# use-ehcache

Ehcache 란 ?

JAVA 진영에서 널리 사용되고 있는 오픈 소스 기반의 Local Cache 이다.

생명주기는 Ehcache 를 사용하는 애플리케이션과 같다.

사용하는 방법은 먼저 의존성을 추가해줍니다.

```kotlin
implementation("org.springframework.boot:spring-boot-starter-cache")
implementation("javax.cache:cache-api:1.1.1")
implementation("org.ehcache:ehcache:3.8.1")
```

그 후 캐싱을 사용하겠다는 Config 파일을 생성해줍니다.

```kotlin
@Configuration
@EnableCaching
class CacheConfig {
}
```

캐싱을 사용할 메소드와 메소드를 사용하는 Controller 를 제작해줍니다.

`@Cacheable` 어노테이션을 이용하여 key 와 조건을 설정합니다. 

```kotlin
@RestController
class DemoController(
    private val demoService: DemoService
) {

    @GetMapping("/get")
    fun get(): Int {
        println("in controller")
        return demoService.get(15L)
    }
}
```

```kotlin
@Service
class DemoService {

    @Cacheable(
        value = ["cache"],
        key = "#root.methodName",
        condition = "#number>10"
    )
    fun get(number: Long?): Int {
        println("in method")
        return 5
    }
}
```

캐싱에 사용할 설정 정보를담은 xml 파일을 생성합니다.

```xml
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns="http://www.ehcache.org/v3"
        xsi:schemaLocation="http://www.ehcache.org/v3 http://www.ehcache.org/schema/ehcache-core-3.0.xsd">

    <cache alias="cache">
        <value-type>java.lang.Integer</value-type>
        <expiry>
            <ttl unit="seconds">30</ttl>
        </expiry>

        <resources>
            <heap unit="entries">2</heap>
            <offheap unit="MB">10</offheap>
        </resources>
    </cache>

</config>
```

application.properties 에 cache config file 위치를 지정해줍니다.

```xml
spring.cache.jcache.config=classpath:ehcache.xml
```

결과

첫번째 호출할 때는 controller, service 두 메소드를 실행하지만

두번째 호출할 때는 캐싱이 되어서 service 메소드는 실행하지 않고 결과를 반환하는 것을 확인할 수 있습니다.

![use-ehcache%207b1e6091f0cf4d168184ee9d7efd1815/Untitled.png](use-ehcache%207b1e6091f0cf4d168184ee9d7efd1815/Untitled.png)