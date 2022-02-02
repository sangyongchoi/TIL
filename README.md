# TIL
## 소개
새로운 개발지식을 알게 될 때 기록하고자 만든 공간입니다.  
언어 제약은 따로 두고있지 않지만 Spring과 관련된 내용을 기록할때는 kotlin 을 이용하고 있습니다.

### kafka

- <a href="https://github.com/sangyongchoi/TIL/blob/master/kafka/introduce.md">kafka basic</a>

### Kotlin
- <a href="https://github.com/sangyongchoi/TIL/blob/master/kotlin/coroutines.md">Coroutines await async</a>
- <a href="https://github.com/sangyongchoi/TIL/blob/master/kotlin/custom-annotation.md">Kotlin 에서 Custom Annotation 사용시 주의사항</a>
- <a href="https://github.com/sangyongchoi/TIL/blob/master/kotlin/factory-function-with-private-constructor.md">객체 생성 시 유효성검사 하는 방법</a>
- <a href="https://github.com/sangyongchoi/TIL/blob/master/kotlin/smart-cast.md">Smart Cast</a>
- <a href="https://github.com/sangyongchoi/TIL/blob/master/kotlin/Coroutine-context-and-dispatchers.md">Coroutine Context VS Dispatcher</a>
- <a href="https://github.com/sangyongchoi/TIL/blob/master/kotlin/Multiple-sort.md">여러가지 조건으로 sort 하기</a>

### Spring

- Spring-Batch
    - <a href="https://github.com/sangyongchoi/TIL/blob/master/spring/batch/We-need-to-use-rowMapper-in-JdbcPagingItemReaderBuilder.md">JdbcPagingItemReaderBuilder 사용시 주의사항</a>
    - <a href="https://github.com/sangyongchoi/TIL/blob/master/spring/batch/AbstractPagingItemReader.md">AbstractPagingItemReader 이 페이징처리를 하는 방법</a>
    - <a href="https://github.com/sangyongchoi/TIL/blob/master/spring/batch/parallel-step.md">Spring Batch 를 이용하여 병렬로 실행하기</a>
- rest docs
    - <a href="https://github.com/sangyongchoi/TIL/blob/master/spring/rest-docs/rest-docs-setting.md">Rest docs 세팅</a>
- Spring Security
    - <a href="https://github.com/sangyongchoi/TIL/blob/master/spring/security/security-filter-exclude-url.md">Custom Filter 등록 후 특정 URL 제외하기</a>
- Spring Data
    - <a href="https://github.com/sangyongchoi/TIL/blob/master/spring/spring-data/%40CreatedDate-Can-be-dangerous.md">@CreatedDate 를 사용하면 안될 때</a>
    - <a href="https://github.com/sangyongchoi/TIL/blob/master/spring/spring-data/Why-use-val-on-entities(kotlin).md">Kotlin에서 Entity 컬럼에 val 을 사용하는 이유</a>
    - <a href="https://github.com/sangyongchoi/TIL/blob/master/spring/spring-data/use-in-query.md">findBy 에서 파라미터로 Collection 을 사용할 때 유의사항</a>
    - <a href="https://github.com/sangyongchoi/TIL/blob/master/spring/spring-data/find-test-with-kotest.md">kotest 를 이용하여 Overload 된 메소드 테스트 할 때 any() 활용법</a>
    - <a href="https://github.com/sangyongchoi/TIL/blob/master/spring/spring-data/use-MongoDB-Transaction.md">Spring Boot 환경에서 MongoDB Transaction 사용하기</a>
    - <a href="https://github.com/sangyongchoi/TIL/blob/master/spring/spring-data/use-mongoOperation-with-querydsl.md">QClass 를 이용하여 Criteria 에 사용되는 필드명 정의하기</a>
    - <a href="https://github.com/sangyongchoi/TIL/blob/master/spring/spring-data/Using-mongodb-type-safe-with-kotlin.md">Kotlin 을 이용하여 Type-safe 하게 MongoDB를 이용하기</a>
    - <a href="https://github.com/sangyongchoi/TIL/blob/master/spring/spring-data/how-to-return-specific-fields-with-MongoOperation.md">MongoOperation 을 사용하여 특정 field 만 조회하기</a>
    - <a href="https://github.com/sangyongchoi/TIL/blob/master/spring/spring-data/how-to-bulkinsert-using-data-mongo%20c303fce96950427b808aeff773adf223.md">여러개의 Document 한번에 insert 하기</a>
- Spring Boot
    - <a href="https://github.com/sangyongchoi/TIL/blob/master/spring/spring-boot/active-profile.md">Spring Boot 2.4 이후버전에서 profile 설정하기</a>    
    - <a href="https://github.com/sangyongchoi/TIL/blob/master/spring/spring-boot/kotest-configuration.md">Spring Boot kotest configuration</a>    
    - <a href="https://github.com/sangyongchoi/TIL/blob/master/spring/spring-boot/enum-converter.md">MongoDB 에 Enum을 저장할 때 Converter 사용하기</a>    
    - <a href="https://github.com/sangyongchoi/TIL/blob/master/spring/spring-boot/use-enum-as-request-param.md">Controller 에서 Enum 타입을 파라미터로 받기</a>    
    - <a href="https://github.com/sangyongchoi/TIL/blob/master/spring/spring-boot/Precautions-when-using-interceptors.md">Interceptor 사용시 주의사항</a>
    - <a href="https://github.com/sangyongchoi/TIL/blob/master/spring/spring-boot/use-ehcache.md">Spring Boot 에서 ehcache 사용하기</a>
    - <a href="https://github.com/sangyongchoi/TIL/blob/master/spring/spring-boot/ResponseClass-Custom-Serialize.md">Custom Annotation을 이용하여 Response 변환하기</a>
    - <a href="https://github.com/sangyongchoi/TIL/blob/master/spring/spring-boot/use-multi-module-with-gradle%201789beec21ce4d3c8d6a9e8a34d6e018.md">gradle multi module 사용하기 (with kotlin)</a>
	
### node
- <a href="https://github.com/sangyongchoi/TIL/blob/master/node/write-node-package-json.md">package.json에서 dependencies 작성방법</a>

### Docker
- <a href="https://github.com/sangyongchoi/TIL/blob/master/docker/DockerFile.md">DockerFile 작성법</a>
- <a href="https://github.com/sangyongchoi/TIL/blob/master/docker/Redis-with-docker.md">Docker를 이용하여 redis 실행하기</a>
	
### MongoDB
- <a href="https://github.com/sangyongchoi/TIL/blob/master/MongoDB/Transaction.md">MongoDB Transaction</a>
- <a href="https://github.com/sangyongchoi/TIL/blob/master/MongoDB/use-indexes-to-sort.md">정렬에서 Index 를 사용하는 방법</a>
- <a href="https://github.com/sangyongchoi/TIL/blob/master/MongoDB/use-indexes-to-sort.md">제일 가까운 위치찾기</a>

### ETC
- <a href="https://github.com/sangyongchoi/TIL/blob/master/etc/transaction-in-MSA.md">MSA 환경에서 분산 트랜잭션 처리하는 방법</a>
- <a href="https://github.com/sangyongchoi/TIL/blob/master/etc/Layered-Architecture.md">Layered Architecture</a>
- <a href="https://github.com/sangyongchoi/TIL/blob/master/etc/Creating-multi-modules(with-IntelliJ).md">IntellJ를 활용하여 Multi-Module 구성하기</a>
