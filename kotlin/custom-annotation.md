# 2021-05-11

kotlin 에서 `property`에서 `custom annotation` 을 사용하려면 `Target Annotation` 을 붙여줘야 한다.

그렇지 않으면 검사를 하지 못한다.

자세한 이유는 알지못해 내일 알아볼 예정이다.

---

reflection 은 비용이 비싼 작업으로 알려져있다.

하지만 spring 에서는 리플렉션을 많이 사용하지만 알려진 성능적인 이슈는 없다.

spring 에서는 최초 1번 실행할 때 reflection  필요한 요소들을 Map 에 캐싱을 해서 쓴다고 한다.

Map 은 시간복잡도가 O(1)이기 때문에 리플렉션을 이용하는 것보다 월등히 빠르다.
