# active-profile

spring boot 2.4 부터는 환경마다 profile 을 다르게 하는 문법이 변경되었다.

기존에는 아래와 같이 작성하였다.

```yaml
spring:
  profiles:
    active: local
```

2.4 이후부터는 아래와 같이 변경된다.

```yaml
spring:
  config:
    activate:
      on-profile: local
```