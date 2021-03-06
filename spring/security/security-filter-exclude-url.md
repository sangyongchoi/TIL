# 2021-05-14

## `Spring Security`

Custom Filter 를 등록하고 특정 URL 제외시키기

CustomFilter 를 만드는 다양한 방법이 있지만 예제 에서는 `OncePerRequestFilter` 를 상속받도록 한다.

`shouldNotFilter` 메소드는 해당 필터를 통과 하는지 아닌지를 검사하는 메소드이다.

제외할 URL 리스트를 만든 후 들어오는 요청이 해당 필터를 타는지 안타는지 검사한다.

```kotlin
class CustomFilter(): OncePerRequestFilter() {

    private val excludeUrlList = listOf("/signup", "/login")

    override fun doFilterInternal(request: HttpServletRequest, response: HttpServletResponse, filterChain: FilterChain) {
        ....
    }

    override fun shouldNotFilter(request: HttpServletRequest): Boolean {
        val path = request.requestURI

        return skipUrlList.contains(path)
    }
}
```

출처 : [https://www.baeldung.com/spring-exclude-filter](https://www.baeldung.com/spring-exclude-filter)
