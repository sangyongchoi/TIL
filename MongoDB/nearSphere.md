# nearSphere

거리기반쿼리를 사용할 때 가장 가까이에 있는것부터 반환하는 명령어이다.

nearSphere 은 geospatial index 가 생성되어있어야 사용이 가능하다.

이번 예제에서는 `2dsphere` 를 사용한다.

아래의 명령어를 이용하여 생성할 수 있다.

```xml
db.places.createIndex({ location: "2dsphere" })
```

sample data 를 넣어준다.

```xml
db.places.insert( { name: "gude", location: {  type: "Point",  coordinates: [ 126.9015400559523, 37.4854279080695 ]  } } );
db.places.insert( { name: "hollys", location: {  type: "Point",  coordinates: [ 126.901154, 37.484755 ]  } } );
db.places.insert( { name: "pizzahut", location: {  type: "Point",  coordinates: [ 126.90110, 27.48323 ]  } } );
```

조회는 아래와 같은 명령어로 할 수 있다.

결과는 제일 가까운 장소부터 나온다.

```xml
db.places.find({ location: { $nearSphere: { $geometry: { type: "Point", coordinates: [ 126.9015400559523, 37.4854279080695  ] }, $maxDistance: 10000 } } })
```