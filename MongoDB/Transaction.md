# Transaction

MongoDB 는 4.0 부터 Multi-document transaction 을 지원한다.

- replica set 이 존재해야만 지원한다.

Multi-documnet transaction 은 WiredTiger storage engine 에서만 사용 할 수 있다.

Transaction 은 Session 에 연결된다.

- 세션당 트랜잭션은 1개만 가능하다.

컬렉션 또는 인덱스 생성 또는 삭제와 같이 데이터베이스 카탈로그에 영향을 미치는 작업은 다중 문서 트랜잭션에서 허용되지 않습니다.

- 트랜잭션을 사용할 데이터베이스, 컬렉션은 미리 생성이 되어있어야 합니다.

트랜잭션의 작업은 transaction-level read concern 을 사용한다.

- collection 이나 database 에서 설정된 read concern 을 무시한다.

트랜잭션의 작업은 transaction-level write concern 을 사용한다.

출처 : [https://docs.mongodb.com/v4.0/core/transactions/](https://docs.mongodb.com/v4.0/core/transactions/)