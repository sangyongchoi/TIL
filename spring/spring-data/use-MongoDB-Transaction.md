# MongoDB Transaction 사용하기

Spring Data Mongo 에서는 Spring 이 제공하는 @Transactional 을 사용할 수 있다.

환경은 아래와 같아야한다.

- MongoDB 4.0 이상
- replica set 환경

```kotlin
@Configuration
class MongoConfig : AbstractMongoClientConfiguration() {
    @Bean
    fun transactionManager(dbFactory: MongoDatabaseFactory?): MongoTransactionManager {
        return MongoTransactionManager(dbFactory)
    }

    override fun getDatabaseName(): String {
        return "test";
    }

    override fun mongoClient(): MongoClient {
        val connectionString = ConnectionString("mongodb://localhost:27017/test")
        val mongoClientSettings: MongoClientSettings = MongoClientSettings.builder()
            .applyConnectionString(connectionString)
            .build()
        return MongoClients.create(mongoClientSettings)
    }
}
```

```kotlin
@Documnet
data class Coke(
   @Id
   val id: ObjectId? = null
)
```

```kotlin
interface CokeRepository : MongoRepository<Coke, ObjectId>
```

```kotlin
@Service
class CokeService(
    private val cokeRepository: CokeRepository
) {

		@Transactional
    fun save() {
				val coke1 = Coke()
				val coke2 = Coke()
			  
				cokeRepository.save(coke1)
				cokeRepository.save(coke2)

				throw RuntimeException("Rollback!!")
    }
}
```

위와 같이 작성했을때 [CokeService.save](http://cokeservice.save) 안의 Exception 이 터지게 되면 coke1, coke2 는 저장되지 않는것을 확인 할 수 있다.

참고 : [https://www.baeldung.com/spring-data-mongodb-transactions](https://www.baeldung.com/spring-data-mongodb-transactions)