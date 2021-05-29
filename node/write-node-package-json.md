# write-node-package-json

module 의존성 관리를 위해 `package.json` 을 작성할 때 주의사항이 있다.

아래와 같이 작성한다면 실행시 `Error: Cannot find module 'express'` 와 같은 오류를 만날 수 있다.

```kotlin
{
  ....,
  "dependencies": {
    "express": "4.17.1"
  }
}
```

아래와 같이 변경을 하면 정상적으로 실행된다.

```kotlin
{
  ....,
  "dependencies": {
    "express": "^4.17.1"
  }
}
```