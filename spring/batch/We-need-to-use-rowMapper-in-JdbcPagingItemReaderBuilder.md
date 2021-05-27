# We-need-to-use-rowMapper-in-JdbcPagingItemReaderBuilder

Spring-Batch 를 사용하려면 ItemReader 와 ItemWriter 를 사용해야한다.

그중 `**JdbcPagingItemReader**` 를 이용할 때 주의사항을 알아보도록 하겠다.

맨 처음에 rowMapper 가 딱히 필요없을 것 같아서 해당 라인을 적지 않았다.

코드는 아래와 같다.

```kotlin
@Bean
fun exampleReader(queryProvider: PagingQueryProvider): JdbcPagingItemReader<Tests>{
    return JdbcPagingItemReaderBuilder<Tests>()
        .name("exampleReader")
        .dataSource(dataSource)
        .queryProvider(queryProvider)
        .fetchSize(jobParamProperties.chunkSize)
        //.rowMapper{ rs, _ -> Tests(rs.getLong("id")) }
        .build()
}

@Bean
fun queryProvider(): PagingQueryProvider {
    val queryProvider = SqlPagingQueryProviderFactoryBean()
    queryProvider.setDataSource(dataSource)
    queryProvider.setSelectClause("id")
    queryProvider.setFromClause("tests")
    queryProvider.setSortKey("id")

    return queryProvider.`object`
}
```

위와 같은 상태로 실행을 하면 NPE 를 만날 수 있었고 그 지점은 org.springframework.batch.item.database.JdbcPagingItemReader$PagingRowMapper.mapRow 에서 발생했다.

원인을 위해 디버깅을 하던 결과 아래와 같은 코드를 보았고 rowMapper 가 등록되지 않아서 NPE 가 발생하던 것이었다.

![We-need-to-use-rowMapper-in-JdbcPagingItemReaderBu%20121fbf1240eb4b39aba0072f38f352cd/Untitled.png](We-need-to-use-rowMapper-in-JdbcPagingItemReaderBu%20121fbf1240eb4b39aba0072f38f352cd/Untitled.png)

우리는 아래와 같이 rowMapper 를 등록해주어야 한다.

```kotlin
@Bean
fun exampleReader(queryProvider: PagingQueryProvider): JdbcPagingItemReader<Tests>{
    return JdbcPagingItemReaderBuilder<Tests>()
        .name("exampleReader")
        .dataSource(dataSource)
        .queryProvider(queryProvider)
        .fetchSize(jobParamProperties.chunkSize)
        .rowMapper{ rs, _ -> Tests(rs.getLong("id")) }
        .build()
}

@Bean
fun queryProvider(): PagingQueryProvider {
    val queryProvider = SqlPagingQueryProviderFactoryBean()
    queryProvider.setDataSource(dataSource)
    queryProvider.setSelectClause("id")
    queryProvider.setFromClause("tests")
    queryProvider.setSortKey("id")

    return queryProvider.`object`
}
```

또 한가지 SqlPagingQueryProviderFactoryBean 를 사용하여 QueryProvider 를 생성할 경우 sort key 를 지정해주지 않으면 에러가 발생한다.