# parallel-step

프로젝트를 진행하다보면 수십분 혹은 몇시간씩 걸리는 배치들이 존재한다.

데이터가 정말 많아서, 혹은 순차적인 작업이 필요해서 등 어쩔 수 없는 이유때문에 오랜시간 걸리는 배치들도 있을 것이다.

하지만, 순차적인 작업이 필요 없는데도 오래걸리는 배치들이 있을것이다.

이 글에서는 그런 배치들을 어떻게 하면 개선할 수 있는지 알아보고자 한다.

일반 게시판을 가진 서비스에서 유저별 게시글, 댓글수, 좋아요수 통계를 매일 새벽 2시에 구한다고 가정해보자.

만약 게시글 수, 댓글 수, 좋아요 수의 작업이 각 10분씩 걸린다고 할 때 순차적으로 진행한다면 총 30분이 걸릴것이다.

게시글 수, 댓글 수, 좋아요 수를 구하는 작업이 순차적으로 이루어져야 하는 작업일까?

순차적으로 이루어져야 하는 경우도 있겠지만, 대다수의 경우 순차적으로 이루어지지 않아도 괜찮을 것이다.

그렇다면 게시글 수, 댓글 수, 좋아요 수를 동시에 구한후에 종합만 한다면 결과는 동일할 것이다.

각 작업을 동시에 실행한다면 작업시간도 10분으로 확 줄어들것이다.

Spring Batch 에서는 이러한 작업을 손쉽게 할 수 있도록 기능을 제공한다.

`SplitBuilder` 를 이용하여 생성한다면 여기에 묶여있는 작업들은 병렬로 실행하게 된다.

예제를 통해 알아보도록 하겠다.

User의 통계정보를 저장하는 `UserStatisticsRepository` 이 있다.

```kotlin
@Component
class UserStatisticsRepository {
    private val allUsersStatistics: HashMap<Int, UserStatistics> = hashMapOf()

    fun add(id: Int) {
        allUsersStatistics[id] = UserStatistics(id = id)
    }

    fun findAll() = allUsersStatistics.toMap()

    fun setPostCount(id: Int, postCount: Int) {
        allUsersStatistics[id]?.postCount = postCount
    }

    fun setCommentCount(id: Int, commentCount: Int) {
        allUsersStatistics[id]?.commentCount = commentCount
    }

    fun setLikeCount(id: Int, likeCount: Int) {
        allUsersStatistics[id]?.likeCount = likeCount
    }
}
```

User 의 총 게시물 수를 세팅하는 Tasklet

```kotlin
@Component
class TotalPostTasklet(
    private val userStatisticsRepository: UserStatisticsRepository,
) : Tasklet {

    override fun execute(
        contribution: StepContribution,
        chunkContext: ChunkContext
    ): RepeatStatus {
        for (i in (1..100)) {
            userStatisticsRepository.setPostCount(i, i)
        }

        return RepeatStatus.FINISHED
    }
}
```

User 의 총 댓글 수를 세팅하는 Tasklet

```kotlin
@Component
class TotalCommentTasklet(
    private val userStatisticsRepository: UserStatisticsRepository,
) : Tasklet{

    override fun execute(
        contribution: StepContribution,
        chunkContext: ChunkContext
    ): RepeatStatus {
        for (i in (1..100)) {
            userStatisticsRepository.setCommentCount(i, i)
        }

        return RepeatStatus.FINISHED
    }
}
```

User 의 총 좋아요 수를 세팅하는 Tasklet

```kotlin
@Component
class TotalLikeTasklet(
    private val userStatisticsRepository: UserStatisticsRepository,
) : Tasklet{

    override fun execute(
        contribution: StepContribution,
        chunkContext: ChunkContext
    ): RepeatStatus {
        for (i in (1..100)) {
            userStatisticsRepository.setLikeCount(i, i)
        }

        return RepeatStatus.FINISHED
    }
}
```

위와 같이 게시물 수, 댓글 수, 좋아요 수를 Set 하는 Tasklet 들이 있을 때, 아래와 같이 설정을 하면, add 메소드 안에 추가된 작업들이 병렬로 실행되게 된다.

그 결과 게시물, 댓글, 좋아요 수를 병렬로 가져오면서 실행시간을 단축시킬 수 있다.

```kotlin
@Bean
fun parallelStep() = FlowBuilder<SimpleFlow>("parallelFlow")
        .split(taskExecutor)
        .add(
            totalPostFlow,
            secondFlow,
            thirdFlow,
        )
        .build()
        .let {
            stepBuilderFactory.get("parallelStep")
                .flow(it)
                .build()
        }
```

위와 같이 병렬로 실행하도록 만든 Step 을 아래와 같이 추가한다.

```kotlin
@Bean
fun usersStatisticsJob(): Job {
    return jobBuilderFactory.get("usersStatisticsJob")
        .start(setUpStep())   // user set up
        .next(parallelStep()) // 병렬로 데이터 set
        .next(mergeStep())    // 만들어진 데이터를 다른곳에 저장 혹은 merge
        .build()
}
```

이렇게 설정을 하게되면 setUpStep 이 끝난이후 parallelStep 이 시작되고 parallelStep 이 끝나고 mergeStep 이 실행된다.