# AbstractPagingItemReader

통계 데이터를 만들 때 배치를 많이 이용한다.

데이터 1억개가 있다고 가정할 때 1억개를 한번에 가져온다면 DB 서버와 배치서버가 매우 고통스러워 할 것이다.

그렇기에 배치에서도 페이징처리를 해주어야한다.

Spring Batch 에서는 페이징처리를 쉽게 할 수 있도록 `AbstractPagingItemReader` 라는 추상클래스를 제공해준다.

그렇다면 `AbstractPagingItemReader` ****는 어떤식으로 페이징 처리를 지원하는지 알아보도록 하겠다.

아래사진은 `AbstractPagingItemReader` 의 구현모습이다.

필드로 pageSize, page 라는 변수를 가지고 있다.

여담이지만 setPageSize 를 이용하여 가져올 데이터의 개수를 지정해줄 수 있다.

![AbstractPagingItemReader%201a0e596b09b24743b6064601f17c01c1/Untitled.png](AbstractPagingItemReader%201a0e596b09b24743b6064601f17c01c1/Untitled.png)

Reader 에서는 doRead 메소드를 이용하여 데이터를 가져온다.

`AbstractPagingItemReader` 의 doRead 메소드에서 페이지를 읽은후에 page 변수를 1씩 증가시켜주면서 페이지를 표현할 수 있도록 도와준다.

![AbstractPagingItemReader%201a0e596b09b24743b6064601f17c01c1/Untitled%201.png](AbstractPagingItemReader%201a0e596b09b24743b6064601f17c01c1/Untitled%201.png)

이상으로 `AbstractPagingItemReader` 가 어떻게 페이징을 도와주는지 살펴보았다.