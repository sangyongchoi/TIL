# use-mongoOperation-with-querydsl

`이 방법은 QueryDSL 이 지원하지 않을때만 사용하는것을 추천드립니다.`

---

MongoOperations 을 사용하기 위하여 Query() 를 사용할 때 조금은 나은방법으로 사용할 수 있는 방법을 소개 하려고 한다.

Query 를 사용할 때는 Criteria 를 사용하여 조건을 명시한다.

아래와 같이 말이다.

```kotlin
Query().addCriteria(
    Criteria
        .where("polygons")
        .intersects(GeoJsonPoint(longitude, latitude))
)
```

이때 where 안에있는 "polygons" 라는 문자열은 오류가 발생하기 딱 좋은 지점이다.

개발자의 실수로 polygosn 라고 적으면 결과가 다르게 나오거나 오류가 발생할 것이다.

QueryDSL 로 Type-safe 하게 개발하면 좋겠지만 QueryDSL 이 지원하지 않는 이유로 그러지 못할 때가 있다.

바로 geoIntersects 가 그런예중 하나이다.

이럴때 최소한 오타로 인한 오류는 안내는 방법을 소개하고자 한다.

QueryDSL 을 사용하게 된다면 Q Class 가 생성되게 된다.

Q Class 의 필드들은 각각 metadata 를가지고 metadata 는 필드의 이름을 가지고 있다.

바로 아래와 같이 말이다.

```java
public final SimplePath<....GeoJsonMultiPolygon> polygons = createSimple("polygons", ....GeoJsonMultiPolygon.class);
```

그리고 저 metadata 가 가지고 있는 필드의 이름은 아래와 같은 방법으로 가져올 수 있다.

```kotlin
polygons.metadata.name
```

이 방법을 이용하여 위의 Query 를 다시 작성한다면 아래와 같은 방법으로 작성 할 수 있다.

```kotlin
Query().addCriteria(
    Criteria
        .where(polygons.metadata.name)
        .intersects(GeoJsonPoint(longitude, latitude))
)
```

이 방법은 Type-safe 하지는 못하지만 오타로 인한 오류는 방지해주므로 쌩 Query 를 사용하는 것보다는 나은 방법이라고 생각한다.

QueryDSL 이 지원한다면 QueryDSL 을 이용하여 개발하는 것을 추천한다.

Type-safe 할 수 있기 때문이다.