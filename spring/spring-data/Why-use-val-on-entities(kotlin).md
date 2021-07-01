# Why-use-val-on-entities(kotlin)

Kotlin 에서는 Entity 를 만들때 아래와 같이 만들 수 있다.

```kotlin
@Entity
data class Member(
  ...
  val 닉네임: String
)
```

이때 Member 의 닉네임은 언제든 바뀔 수 있다.

이런 상황에서 val 을 사용하게 되면 setter 가 만들어지지 않으므로 값을 수정할 수 없게된다.

수정할 수 없으므로 JPA 의 변경감지를 통한 update 쿼리를 사용할 수 없게된다.

그렇다면 그러한 이유로 var 를 사용하는 것이 옳을까 ?

코틀린에서는 var 을 사용하게 되면 setter 가 자동으로 생기므로 외부에서 값을 변경할 수 있다.

JPA 와 함께 사용하게되면 DB의 값까지 자동으로 update 가 될 수 있다.

즉, 의도하지 않은 update 쿼리가 발생할 수 있는 위험이 있다.

그렇기에 val 로 고정하고 별도의 update 쿼리를 통해 값을 변경하는 방법을 사용할 수 있다.