# transaction-in-MSA

> 공부중인 주니어개발자가 작성하는 글입니다.
틀린 내용이 존재할 수 있으며 무조건적인 신뢰보다는 참고정도만 해주시면 감사하겠습니다.
틀린부분을 지적해주셔도 좋습니다.
> 

MSA 환경에서는 트랜잭션 처리가 중요하면서 까다롭습니다.
MSA 환경에서는 각 서비스마다 저장소에 대한 접근권한을 따로 가지고 있을 확률이 높습니다.
이것이 무슨뜻인지 풀어서 설명해보겠습니다.
User Service 와 Point Service 가 있다고 가정해보겠습니다.
User Service 에서 사용하는 저장소는 User Service 에서만 접근할 수 있습니다.
또한, Point Service 에서 사용하는 저장소는 Point Service 에서만 접근할 수 있습니다.
User Service 와 Point Service 가 엮여있는 상황이 있고 user table 과 point table 의 트랜잭션이 한 트랜잭션처럼 처리를 하려면 어떻게 해야할까요 ? 
보편적으로 크게 2가지 방법이 사용됩니다.

1. Rest API 활용
2. event 활용

이 예제에서는 event 를 활용하여 트랜잭션을 처리하는 방법을 알아보도록 하겠습니다.

말도 안되는 비즈니스 요건이지만 예제를 위해 아래와 같은 요건이 있다고 생각해봅시다.

1. User 회원가입
2. 가입 축하 Point 지급
    1. 성공했다면 → 정상 회원가입
    2. 실패했다면 → 탈퇴처리

그렇다면 플로우는 아래와 같이 될 것입니다.

1. User 회원가입 → Point Service 호출 → Point 지급 시도 → 결과에 따른 처리 (회원가입 혹은 탈퇴)

![Untitled](transaction-in-MSA%209bee573ee0724e67978a5d3a1b06e5d5/Untitled.png)

유저가 회원가입을 하게되면 User Service 를 통해 User Table 에 저장을 합니다.
그 후 `회원가입` 이벤트를 발행합니다. 
Point Service 는 `회원가입` 이벤트가 발행되면 가져다가 가입한 유저에게 포인트를 주려는 시도를 합니다.
그 후 성공을 했다면 `회원가입 성공` 이벤트를, 실패했다면 `회원가입 실패(롤백)` 이벤트를 발행합니다.
User Service 는 `회원가입 성공` 이벤트와 `회원가입 실패(롤백)` 이벤트가 발행되면 알맞는 로직을 실행합니다. 

이런식으로 이벤트를 통해 트랜잭션을 관리할 수 있습니다.

예제 소스

[https://github.com/sangyongchoi/saga-example](https://github.com/sangyongchoi/saga-example)

참고자료

[https://microservices.io/patterns/data/saga.html](https://microservices.io/patterns/data/saga.html)

[https://chrisrichardson.net/post/sagas/2019/08/15/developing-sagas-part-3.html](https://chrisrichardson.net/post/sagas/2019/08/15/developing-sagas-part-3.html)