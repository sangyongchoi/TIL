# use-indexes-to-sort

Mongo 에서는 인덱스에 정렬필드가 포함되어 있다면 인덱스를 사용하여 정렬을 할 수 있다.

인덱스에  포함되지 않은경우 blocking sort 를 사용한다.

blocking sort는 collection 또는 database 에서 concurrent 작업을 차단하지 않는다.

공식문서에 따르면 index 를 이용한 정렬이 blocking sort 보다 성능이 좋다.

정렬은 역방향, 정방향 둘 다 지원한다.

예를 들어보자

아래와 같은 인덱스를 생성 했을 때

```xml
db.records.createIndex( { a: 1 } )
```

 두 쿼리 모두 위의 인덱스를 사용한다.

```xml
db.records.find().sort( { a: 1 } )
db.records.find().sort( { a: -1 } )
```

하지만 Multiple field 를 가진 인덱스는 순서가 중요하다.

예를 들어보자

아래와 같은 인덱스를 생성 했을 때

```xml
db.records.createIndex( { a: 1, b: 1 } )
```

오로지 a, b 순서로만 정렬이 가능하다. b,a 의 순서로는 정렬할 수 없다.

또 다른 예를 들어보자

아래와 같은 인덱스를 생성 했을 때

```xml
db.records.createIndex( { a: 1, b: -1 } )
```

아래와 같은 패턴으로 정렬이 가능하다.

1. a:1 b:-1
2. a:-1 b:1

아래와 같은 패턴은 불가능하다.

1. a: 1, b: 1
2. a: -1, b: -1

또한, 정렬에 사용하는 키가 인덱스 키거나 인덱스의 prefix 인경우 해당 인덱스를 사용할 수 있다.

예를 들어보자

아래와 같은 인덱스를 생성 했을 때

```xml
db.data.createIndex( { a:1, b: 1, c: 1, d: 1 } )
```

아래와 같은 prefix 가 생성된다.

```xml
{ a: 1 }
{ a: 1, b: 1 }
{ a: 1, b: 1, c: 1 }
```

정렬에 사용되는 키가 prefix가 아닐때도 index(또는 prefix) 를 사용하여 정렬할 수 있다.

하지만 쿼리에 index(또는 prefix) 에 해당하는 키가 존재해야 한다.

예를 들어보자

```xml
db.data.find( { a: 5 } ).sort( { b: 1, c: 1 } )
```

위의 쿼리같은경우 정렬에 사용되는 b, c 는 위에서 생성한 index(prefix) 가 아님에도 불구하고 index 를 사용한 정렬이 가능하다. 이유는 쿼리에서 a 컬럼을 사용하면서 결국 a, b, c 를 사용하기 때문에 prefix 를 만족하기 때문이다.

예를 하나만 더 보도록 하겠다.

```xml
db.data.find( { b: 3, a: 4 } ).sort( { c: 1 } )
```

위의 쿼리도 정렬에 사용하는 c 는 위에서 생성한 index(prefix) 가 아니지만 index 를 사용하여 정렬한다.

이유는 마찬가지로 쿼리에서 a, b 를 사용하여 결국 a, b, c 를 사용하기 때문에 prefix 를 만족하기 때문이다.

출처 : [https://docs.mongodb.com/manual/tutorial/sort-results-with-indexes/](https://docs.mongodb.com/manual/tutorial/sort-results-with-indexes/)