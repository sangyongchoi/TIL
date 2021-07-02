# Precautions-when-using-interceptors

**Interceptor 란 ?**
특정요청에 대한 요청을 가로채서 수행하기 전, 수행한 후 추가적인 작업을 할 수 있도록 해주는 것

- preHandle : 실제 handle 이 실행되기 전 수행
- postHandle : 실제 handle 이 수행된 후 실행
- afterCompletion : 요청이 완료된 후 실행 ( View Render 가 완료된 후 )

적용 방법은 간단하다.

아래와 같은 Interceptor 를 만든 후 등록해준다.

```kotlin
@Component
class DemoInterceptor : HandlerInterceptor {

    override fun preHandle(request: HttpServletRequest, response: HttpServletResponse, handler: Any): Boolean {
        println("before handle")
        return true
    }

    override fun postHandle(
        request: HttpServletRequest,
        response: HttpServletResponse,
        handler: Any,
        modelAndView: ModelAndView?
    ) {
        println("after handle")
    }

    override fun afterCompletion(
        request: HttpServletRequest,
        response: HttpServletResponse,
        handler: Any,
        ex: Exception?
    ) {
        println("complete handle")
    }
}
```

```kotlin
@Configuration
class WebConfig(
    private val demoInterceptor: DemoInterceptor
) : WebMvcConfigurer {

    override fun addInterceptors(registry: InterceptorRegistry) {
        registry
            .addInterceptor(demoInterceptor)
            .addPathPatterns("/**")
    }
}
```

결과는 어떻게 동작 하는지 확인해보자.

```kotlin
@RestController
class DemoController {

    @GetMapping("/demo")
    fun demo(): ResponseEntity<String> {
        return ResponseEntity.ok("success")
    }
}
```

/demo 로 요청을 보내면 아래와 같이 동작한다.

![Precautions-when-using-interceptors%201b92f0d392b7404cb5c70aabad0cfa38/Untitled.png](Precautions-when-using-interceptors%201b92f0d392b7404cb5c70aabad0cfa38/Untitled.png)

하지만 주의해야할 점이 있다.

postHandle 메소드는 Handler 실행중 Exception 이 발생하면 동작하지 않는다.

```kotlin
@RestController
class DemoController {

    @GetMapping("/demo-exception")
    fun demoException(): ResponseEntity<String> {
        throw RuntimeException("exception")
        return ResponseEntity.ok("success")
    }
}
```

/demo-exception 으로 요청을 보내면 무조건 Exception 이 발생하도록 코딩했다.

결과는 아래와 같다.

![Precautions-when-using-interceptors%201b92f0d392b7404cb5c70aabad0cfa38/Untitled%201.png](Precautions-when-using-interceptors%201b92f0d392b7404cb5c70aabad0cfa38/Untitled%201.png)

그 이유는 Spring 의 DispatcherServlet 을 보면 handler 를 실행하기 전( ha.handle(...) ) 에 preHandle 을 실행하고 handler 를 실행 한 후에 postHandle 을 실행한다.

handler 를 실행하다가 exception 이 발생되면 catch 쪽으로 빠지므로 postHandle 이 실행되지 않는다.

afterCompletion 는 실행되는 이유는 try {} catch {} 의 범위 밖에서 실행되기 때문이다.

this.processDispatchResult 안에서 afterCompletion 실행된다.

![Precautions-when-using-interceptors%201b92f0d392b7404cb5c70aabad0cfa38/Untitled%202.png](Precautions-when-using-interceptors%201b92f0d392b7404cb5c70aabad0cfa38/Untitled%202.png)