# how-to-bulkinsert-using-data-mongo

다수의 도큐먼트를 insert 할때 1개씩 넣어주는것보다 한번에 여러개를 넣어주는것이 효율적이다.

100개의 도큐먼트가 있다고 가정할때 100번의 네트워크 비용보다 1번의 네트워크 비용이 저렴하기 때문이다

MongoDB에는 다수의 Document를 한번에 저장할 수 있는 insertMany 라는 기능을 지원한다.

Spring-data-mongo 에서는 insertMany 를이용하여 여러개의 Document를 한번에 저장할 수 있는 saveAll 이라는 Method 를 제공한다.

아래는 saveAll 메소드의 구현내용이다.

내용을 본다면 전부 새로운 Document 여야 saveAll 기능을 효과적으로 이용할 수 있다는 것을 확인할 수 있다.

주의하도록 하자!

```java
public <S extends T> List<S> saveAll(Iterable<S> entities) {

		Assert.notNull(entities, "The given Iterable of entities not be null!");

		Streamable<S> source = Streamable.of(entities);
		boolean allNew = source.stream().allMatch(entityInformation::isNew);

		if (allNew) {

			List<S> result = source.stream().collect(Collectors.toList());
			return new ArrayList<>(mongoOperations.insert(result, entityInformation.getCollectionName()));
		}

		return source.stream().map(this::save).collect(Collectors.toList());
	}
```

사용방법은 굉장히 간단하다.

아래와 같이 사용하면 된다.

```kotlin
@Document
data class User(
  @Id
	val id: ObjectId? = null,
	val name: String,
)

interface UserRepository : MongoRepository<User, ObjectId>
```

```kotlin
val users = listOf(
	User(name = "test1"),
	User(name = "test2"),
	User(name = "test3"),
)

userRepository.saveAll(users)
```